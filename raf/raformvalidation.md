## Reactive Form Validation

#### Simple Validation
```
control = new FormControl('', [
    Validators.required,
    Validators.minLength(5),
    Validators.email
]);
```
#### Html Template
```
<label for="email">My email is: </label>
<input type="email" id="email" [formControl]="control" />

<pre>errors: {{ control.errors | json }}</pre>
<pre>status: {{ control.status | json }}</pre>

```

#### Validation
```
stats = ['attack', 'defense', 'speed', 'health'];
form = new FormGroup({
    name: new FormControl('', Validators.required),
    stats: new FormGroup({
      attack: new FormControl(0, statValidators),
      defense: new FormControl(0, statValidators),
      speed: new FormControl(0, statValidators),
      health: new FormControl(0, statValidators)
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

<hr />

<table>
  <tr>
    <th>Control</th>
    <th>errors</th>
    <th>status</th>
  </tr>
  <tr>
    <td>[parent]</td>
    <td>
      <pre>{{ form.errors | json }}</pre>
    </td>
    <td>
      <pre>{{ form.status }}</pre>
    </td>
  </tr>
  <tr>
    <td>name</td>
    <td>
      <pre>{{ form.get('name').errors | json }}</pre>
    </td>
    <td>
      <pre>{{ form.get('name').status }}</pre>
    </td>
  </tr>
  <tr>
    <td>stats</td>
    <td>
      <pre>{{ form.get('stats').errors | json }}</pre>
    </td>
    <td>
      <pre>{{ form.get('stats').status }}</pre>
    </td>
  </tr>
  <tr *ngFor="let stat of stats">
    <td>stats.{{ stat }}</td>
    <td>
      <pre>{{ form.get('stats').get(stat).errors | json }}</pre>
    </td>
    <td>
      <pre>{{ form.get('stats').get(stat).status }}</pre>
    </td>
  </tr>
</table>

```


#### Screen Validation
```
form = new FormGroup({
    input: new FormControl('', [Validators.required, Validators.maxLength(10)])
});
```
#### Html Template
```
<form [formGroup]="form" (submit)="submit()">
  <div class="form-group">
    <label for="word">Some Less Than 10-Character Input</label>
    <input
      id="word"
      type="text"
      formControlName="input"
      class="form-control"
      [class.is-invalid]="form.invalid"
      [class.is-valid]="form.valid"
    />
    <div *ngIf="form.invalid" class="invalid-feedback">
      <div *ngIf="form.get('input')['errors'].required">
        Some input is required.
      </div>
      <div *ngIf="form.get('input')['errors'].maxlength as maxlength">
        Input must be less than {{ maxlength.requiredLength }} characters.
      </div>
    </div>
  </div>
  <button type="submit" [disabled]="!form.valid" class="btn btn-primary">
    Submit
  </button>
</form>
```