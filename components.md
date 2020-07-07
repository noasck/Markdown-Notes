#  ViewChild

### HTML

Usage: #<id>

### Import

from '@angular/core'

ElementRef from '@angular/core'

### TS

```typescript
@ViewChild('', {static: false}) inputRef:ElementRef
```

inputRef.**nativeElement** = *common DOM object*

# Receive HTML

### ngContent & ngTemplate

``` html
 <ng-template> Receives data from template </ng-template>
 <ng-content> Creates reference ... </ng-content>
```

### Content Child

``` typescript
// from '@angular/core' Доступ до елементов ng-content

@ContentChild('<id>', {static: true\false}) infoRef;

infoRef.nativeElement //DOM ELEMENT
```

# Angular Lifecycle Hooks

Angular.io -> lifecycle hooks article *(more on docs)*. Constructor called before **ngOnChanges()**  **ngOnInit()**

### changeDetection: ChangeDetectionStrategy.OnPush()

> Ангуляр будет реагировать только на входные свойства Input(). Например, при изменении ссылки на объект. Выгодно использовать для объектов, которые зависят только от входных параметров.