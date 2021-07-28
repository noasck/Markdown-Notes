## Numeric pipes
####Number
``` 
<p>
{{ variable | number:'before_point.range-afterPoint' }}
</p>
```
## String pipes
```
<html>

{{ some_str | uppercase }}
{{ some_str | lowercase }}
{{ some_str | titlecase }}
{{ some_str | slice:1:5 }} #in range from 1 to 5th char (indexing from 1)

</html>
```
## DateTime pipes

```
date = new Date()

<html>
{{ date | date : format : timezone : locale }}
Example:
{{ date | date : 'fullDate'}}
</html>
```

## Creating pipe
``` bash
# Creating with ng CLI
ng g p pipes/<name> --skipTests
```

## Filter lists with pipes
``` html
  <input type="text" [(ngModel)]="search">
  <div
    *ngFor="let post of posts | filter:search"
  >
    <h2>{{post.title}}</h2>
    <p>{{post.text}}</p>
  </div>
```

#### Filter pipe
``` typescript
import {Pipe, PipeTransform} from '@angular/core'
import {Post} from '../app.component'

@Pipe({
  name: 'filter'
})

export class FilterPipe implements PipeTransform {
  transform(posts: Post[], search: string = '', field: string = 'title'): Post[] {
    if (!search.trim()) {
      return posts
    }
    return posts.filter(post => {
      return post[field].toLowerCase().includes(search.toLowerCase())
    })
  }
}
```

## Disable pipe optimization - makes tracking updates
```
@Pipe({
  name: 'pipeName',
  pure: false
})
```

## Async pipe - Rx.js insctances
```
Wait for it... {{ p | async }} // tracking response
```