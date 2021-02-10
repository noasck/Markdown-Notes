# Подводные камни asyncio

2 простых правила: тыкаем ```async\await```.

``` python
async def f():
    return await g()
```

## await = yield point
Программа работает от ```await``` до ```await```. На yield point управление может перейти от одной леговесной задачи к другой, но  ```await``` - не гарантия переключения.

Если между ```await``` закрадётся долгий синхронный вызов, программа зависнет.

Чисто формально, сделать полное покрытие тестами асинхронного кода **практически невозможно**. Юнит тесты проверяют логику, но являются неполными по определению.

## Blocking calls:

#### Wrong:
``` python
async def f():
    return calc_pi(1000)
```
Пока выполняется синхронный вызов, event loop и механизм работы корутин замирают.

### Right:

``` python
from functools import partial

async def f():
    # creates function call with params
    call = partial(calc_pi, 1000, {"params":"value"})

    # running in threaded pool
    loop = asyncio.get_event_loop()
    return await loop.run_in_executor(None, call)
```
Из синхронной функции получаем асинхронный вызов в пуле потоков. Главный поток с event loop'ом функционируют независимо.

## Lightweight tasks
Можем запускать задачи с помощью: ```asyncio.gather(*list_of_tasks)```.

## Debug mode

``` bash
PYTHONASYNCIODEBUG=x python x.py
```

## Timeouts

Почему стоит помнить о таймаутах? Сеть неустойчива и ненадёжна. При разработке ошибки не видны. Стоит учитывать постоянную угрозу прерывания соединения. Ждать дефолтный системный таймаут слишком долго.

``` python
async with async_timeout.timeout(10):
    await gather(url)
```
Аккуратно проставить таймауты очень сложно. Таймауты должны быть зашиты в библиотеках. Все библиотеки, для любого клиента, должны иметь разумный таймаут.

Таймауты должны обрабатываться либо на верхнем уровне:

``` python
async with timeout(10):
    await asyncio.gather(*tasks)
```
Либо можно ставить таймауты внутри каждой задачи и указать параметром ```gather``` **return_exceptions=True**:
``` python
await asyncio.gather (*tasks, return_exceptions=True)
```

## Task cancellation

*Very implicit and deadly unexpected.*

Задачи, порождённые потоки отменяются постоянно. Пример, **aiohttp** handler:

``` python
async def handler(request):
    db = request.app['db']
    async with db.acquire() as connection:
        await connection.execute("INSERT .  .  . ")
```
#### Hint
В данном примере нужно использовать ```asyncio.shield()```, хотя есть более удобный способ.

В процессе обработки риквеста мы делаем запись в бд. При закрытии соединения ```handler``` **прекратит работу** в следствии особенностей работы протокола http. Иными словами, задача может сложиться в любом месте.

Почему это важно? На ошибки и нестабильности в работе сети нужно реагировать. Если результат задачи больше не требуется(соединение разорвано), то работающую логику нужно правильно свернуть(*graceful shutdown*) и освободить ресурсы для других задач.

## AIOJOBS
Вместо применения ```shield``` можно использовать подход **Fire and Forget**.

``` python
loop.create_task(worker(params))
```
Но есть связанные проблемы:
- No error handling.
- No limits for tasks number. Numerous tasks may slow down the server.
- No graceful shutdown.

Другой подход - ждать задачу:

``` python
task = asyncio.create_task(worker(params))
.  .  .
await task
```

Однако, обработка резултата в таком случае всё равно невозможна внутри хэндлера, ввиду возможности обрыва соединения.

Решение проблемы - библиотека ```aiojobs```. Далее описаны принципы, которые необходимо соблюсти, и их непосредственная реализация.

## Safe code

> Бесконечность у нас только в математике. Программирование это инженерная наука, тут нужны какие-то лимиты © Андрей Светлов

#### Main principles
- Control amount of spawned tasks
- Process their exceptions
- Close spawned but not finished yet tasks properly

Для защищённого выполнения задач применяется такой подход:

``` python
async def coro(timeout):
    """ Example of task. """
    await asyncio.sleep(timeout)

async def main():
    scheduler = aiojobs.create_scheduler()
    for i in range(100):
        # spawn jobs
        await scheduler.spawn(coro(i/10))
    await asyncio.sleep(5.0)
    # not all scheduled jobs are finished at the moment

    # graceful close spawned jobs
    await scheduler.close()
```
По умолчанию ёмкость **scheduler** - 100 задач. Этот параметр нужно угадать.

#### В aiohttp
Для создания новой задачи для выполнения логики, к хэндлеру крепится декоратор ```atomic```

## Async design hints

### 1. Hidden constructors are dangerous.

##### Wrong:
``` python
class A:
    client = aiohttp.ClientSession()
```
При таком подходе захватывается глобальный event loop. Это плохо сказывается на тестированию. **Юнит тесты должны создавать event loop для 1 теста** и по окончанию его гасить. Иначе, exeptions из порождённых, но незавершенных процессов будут накладываться между тестами. Таким образом, будет складываться ошибочная картина о происхождении ошибки.
##### Right:
``` python
client = await aiohttp.create_session()
```
### 2. Split brain.

##### Wrong:
``` python
reader, writer = await asyncio.open_connection(host, post)
```

##### Right:
``` python
stream = await open_connection(host, post)
```
> Мы делаем код для коллег. В первую очередь он должен быть понятным. Если уж сильно хочется странного - есть семейство языков типо BrainFuck, и вот там можете крутить самые разнообразные вещи. Получая естетическое удовлетворение. На питоне пусть будет проще и очевиднее. © Андрей Светлов

### 3. Async functions everywhere.

##### Wrong:
``` python
writer.write(b"data")
writer.drain()
```
Мы делаем синхронную запись, которая попадёт в сокет, либо в буфер. Недостаток:
- Синхронный код.
- 2 функции нужно всегда склеивать, вызывать последовательно.

Drain не ждёт пока данные уйдут в буфер. Как только достигнут верхний лимит объёма буфура(High watermark), *asyncio* ждёт, пока загрузка не опустится ниже нижнего лимита. Успешная запись в *asyncio* не означает, что данные ушли в ядро. Ошибки нужно одинаково обрабатывать, независимо от места сбоя на маршруте следования данных.

##### Right:
``` python
await stream.write(b"data")
```
### 4. Close() should be async.

##### Wrong:
``` python
writer.close()
```
Проблема в том, что соединение будет закрыто только на следующей итерации event loop'a. На следующей строчке, ```writer``` ещё не будет полностью закрыт.
##### Much better:
``` python
server.close()
await server.wait_closed()
```
Проблема: всё должно быть одним вызовом. Такой подход неэфективен.
##### Best:
``` python
await stream.close()
```
