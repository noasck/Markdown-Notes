Существует только 2 вида импортов
``` typescript
import {} from 'rxjs'
import {} from 'rxjs/operators'

// Пример
import {interval} from 'rxjs'

export class AppComponent{
	constructor() {
		// Оборачиваем в РхДжс стрим
		const IntervalStream$ = interval(1000)
		// Подписываемся на событие
		this.sub = intervalStream$.subscribe( (value) => /* callback */ {});
	}
	// Optimization
	 stop(){  this.sub.unsubscribe()  }
}
```
## Operators
```
import {map, filter} from 'rxjs/operators'
//  ...
	const IntervalStream$ = interval(1000)
	this.sub = intervalStream$
		.pipe(	
				filter( value => value % 2 === 0),
				map ( (value) => `To String ${value}`)
				)
		.subscribe( (value) => {}); //chain
//  ...
```

## Observable
``` typescript
import {Component} from '@angular/core'
import {Subscription, Observable} from 'rxjs'
import set = Reflect.set

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})
export class AppComponent {

  sub: Subscription

  constructor() {
    const stream$ = new Observable(observer => {

      setTimeout(() => {
        observer.next(1)
      }, 1500)


      setTimeout(() => {
        observer.complete()
      }, 2100)

      setTimeout(() => {
        observer.error('Something went wrong')
      }, 2000)


      setTimeout(() => {
        observer.next(2)
      }, 2500)

    })

    this.sub = stream$
      .subscribe(
        value => console.log('Next: ', value),
        error => console.log('Error: ', error),
        () => console.log('Complete')
      )
  }


  stop() {
    this.sub.unsubscribe()
  }
}

```

## Subject
``` typescript
stream$: Subject<void> = new Subject<void>();
sub: Subscription
constructor(){
	this.sub = this.stream$.subscribe(
		value => {
			// some async with value
		}
	)
	
	next(){
	this.stream$.next(value)
	}
}
```