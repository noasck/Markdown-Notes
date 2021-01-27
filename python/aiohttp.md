# Aiohttp от автора.

**Aiohttp** - async HTTP-library for *asyncio*.

Use cases:
- Microservices
- Websockets
- Non HTTP-transports
- Long-running tasks
-----------------------

## Asyncio remark

- A kind of **concurrent** programming.
- Asyncio task is a **lightweight thread**.
- Utilize the knowledge of multithreaded approach.
- Use locks, events, queues etc. *No low level API.*

Aiohttp не нужно использовать внутри синхронных WSGI фреймворков. Написать надёжно сложно. Asyncio, aiohttp не были написаны для этих случаев.

## Fire and forget - плохо

``` python

async def process(url):
    .  .  .

async def process_all():
    for url in urls:
        asyncio.create_task(process(url))

```

#### Main problems
- Errors not handled
- Amount of spawned task is not controlled
- Graceful shutdown is impossible


## IO in constructor? Use factory method.

``` python
class Cls:
    def __init__(self):
        self.client = None

    @classmethod
    async def create(cls):
        self = cls()
        self.client = await aiohttp.ClentSession()

obj = await Cls.create()
```

## Aiohttp client
#### Client Session
Reuse session - ```import``` everywhere + ``` session.close()``` at the end.

Этот подход сохраняет время на создании экземпляра и установке соединения с сервером.
#### Timeouts
- total (5 min default)
- connect - включает время ожидания получения соединения из пула.
- sock_read
- sock_connect - только время соединения.

## Task cancellation
Есть некоторая задача, выполняемая внутри хэндлера:
``` python
 async def handler(request):
     await task()
     return web.Response(test="OK")
```
Задача прекратится как только будет разорвано соединение со стороны клиента. Это особенности работы протокола HTTP.

Решение:
- aiojobs
- shield

## Inheritance

Не стоит наследоваться от внутренних классов библиотек.
