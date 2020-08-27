Сервисы - связывают данные с отображением. Хорошей практикой является добавление декоратора *Injectable* к сервису. Можно инжектировать любые сущности. 

``` typescript
import {Injectable} from '@angular/core'

@Injectable({providedIn: 'root'}) // регистрируем в корневом модуле

export class AppCounterService {
  counter = 0

  increase() {
    this.counter++
  }

  decrease() {
    this.counter--
  }
}
```
## Создание сервиса через Ng-CLI

``` bash
ng g s services/<service-name> --skipTests
```
## Область видимости сервиса
При создании нового компонента counter. **Injectable 'root'** - изменяется одновременно всеми компонентами. Импортируемый в модуле сервис по сути является синглтоном. Сервис доступен у всех детей регистрирующего компонента. Локальный сервис работает на уровне компонента (с помощью импорта в *providers*). 

