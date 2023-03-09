## Reactive Form

#### Creating a Simple Reactive Form Control
```
control = new FormControl('Joe');
```
#### Checking Value Changes
```
this.control.valueChanges.pipe(
tap(value => console.log(value)),
takeUntil(this._destroying$)
)
```
#### Set Control Value
```
this.control.setValue('Zack');
```
#### Enab/Disable Control
```
//Enable/Disable Control
this.control.enable();
this.control.disable();
```
#### Html Template
```
<input type="text" [formControl]="control" />
{{control.value}} 
```
