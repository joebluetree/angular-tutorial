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



#### Nested From Group
```
form = new FormGroup({
    name: new FormControl(''),
    stats: new FormGroup({
      attack: new FormControl(0),
      defense: new FormControl(0),
      speed: new FormControl(0),
      health: new FormControl(0)
    })
});

```

#### Html Template
```
<form [formGroup]="form">
  <fieldset>
    <legend>Hero Form</legend>

    <label for="name">Hero Name: </label>
    <input type="text" formControlName="name" id="name" />

    <form formGroupName="stats">
      <fieldset>
        <legend>Hero Stats</legend>

        <div>
          <label for="attack">Attack: </label>
          <input type="number" formControlName="attack" id="attack" />
        </div>

        <div>
          <label for="defense">Defense: </label>
          <input type="number" formControlName="defense" id="defense" />
        </div>

        <div>
          <label for="speed">Speed: </label>
          <input type="number" formControlName="speed" id="speed" />
        </div>

        <div>
          <label for="health">Health: </label>
          <input type="number" formControlName="health" id="health" />
        </div>
      </fieldset>
    </form>
  </fieldset>
</form>
```

#### css 
```
legend {
  background-color: #000;
  color: #fff;
  padding: 3px 6px;
  text-align: center;
}

fieldset {
  border-width: 3px;
  border-style: solid;
  border-radius: 5px;
  margin: 10px;
  padding: 10px;
}
```