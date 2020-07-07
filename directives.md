##  Sample directive declaration

``` typescript
// From @angular/core - Directive
@Directive({
 selector: '[appStyle]'
})
 export class DirectiveClass // + import from module.ts
{
 @Input('appStyle') param : string = default
 // Sends param from html
 @Input() fontWeight = 'normal'
 //Sending object
 @Input() dStyles: { border? : string, fontWeight?: string, borderRadius?: string }
 constructor(private elRef: ElementRef, private renderer: Renderer2) {
 this.renderer // some usefull methods
 // Example:
 this.elRef.nativeElement //call DOM element
 } //injection
 // -----------------------[ DYNAMICS: ]------------------------
 @HostListener('click', [ '$event' ]) onClick(event: Event){
 //event model
 }
 @HostListener('onMouseEnter', ['$event.target']){
 // Example : native element style change
 this.renderer.setStyle(this.elRef.nativeElement, 'color', 'blue')
 elRef.nativeElement
 }
 }
```

``` html
XAMPLE USAGE:
<div appStyle="param" fontWeight='bold' 
[dStyles]="{border: '1px solid blue', borderRadius: '5px'}" ...>
```
Without binding -- **string** data type
### Directive generation with Angular CLI

```bash
ng g d style2 --skipTests #generates directive
# declare in app.module declarations
```

### HostBinding from '@angular/core'

```typescript
//INSIDE: DirectiveClass
// Example: changes style, class
@HostBinding('style.color') elColor 

//INSIDE: event:
this.elColor = //changing atribute
```

## Structure directive

Changes DOM HTML structure

```html
EXAMPLE:

<div *appifnot="condition">
Some content
</div>
```

```typescript
@Directive({ selector: '[appStyle]'}) 

export class IfnotDirective {

  @Input('appIfnot') set ifNot(condition: boolean) {
    if (!condition) {
      // Показать элементы
      this.viewContainer.createEmbeddedView(this.templateRef)
    } else {
      // Скрыть
      this.viewContainer.clear()
    }
  }

  constructor(private templateRef: TemplateRef<any>,
              private viewContainer: ViewContainerRef) { }

}
```