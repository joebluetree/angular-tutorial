## Reactive Form Group

#### Creating Form Group
```
form = new FormGroup({
  code: new FormControl(false),
  name: new FormControl('')
});

```
#### Html Template
```
<form [formGroup]="form">
 <input type="text" formControlName="code"/>
 <input type="text" formControlName="name"/>
 <pre>Form Value: {{ form.value | json }}</pre>
</form>
```

