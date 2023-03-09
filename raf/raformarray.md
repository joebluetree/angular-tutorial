## Reactive Form Array

#### Creating Form Array
```
form = new FormArray(
  [new FormControl('')]
);
addControl() {
  this.form.push(new FormControl(''));
}
removeControl(index: number) {
  this.form.removeAt(index);
}
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

####  Form Array
```
const createHero = (): Hero => ({
  name: '',
  stats: {
    attack: 0,
    defense: 0,
    speed: 0,
    health: 0
  }
});

const createHeroForm = (hero?: Hero): FormGroup =>
  new FormGroup({
    name: new FormControl(hero ? hero.name : ''),
    stats: new FormGroup({
      attack: new FormControl(hero ? hero.stats.attack : 0),
      defense: new FormControl(hero ? hero.stats.defense : 0),
      speed: new FormControl(hero ? hero.stats.speed : 0),
      health: new FormControl(hero ? hero.stats.health : 0)
    })
});
possiblePartySizes = [1, 2, 3, 4, 5, 6];
form = new FormGroup({
    name: new FormControl(''),
    partySize: new FormControl(3),
    heroes: new FormArray([])
});

```
