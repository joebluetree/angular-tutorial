## Reactive Form Array

#### Creating Form Array
```
form = new FormArray(
  [new FormControl('')]
);
// Add record to array
this.form.push(new FormControl(''));
// Remove record from array
this.form.removeAt(index);
```

#### Html Template
```
<div *ngFor="let control of form.controls; let i = index">
  <label [for]="'control-' + i">Item {{ i }}: </label>
  <input type="text" [id]="'control-' + i" [formControl]="control" />
  <button (click)="removeControl(i)">Remove</button>
</div>
<button (click)="addControl()">Add New Control To The Array</button>
<pre>Form Array value: {{ form.value | json }}</pre>
```
