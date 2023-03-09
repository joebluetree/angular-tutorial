## Control Value Accessor

#### Creating Control Value Accessor
```
@Component({
  selector: 'forms-course-lesson1-completed-toggle-form',
  templateUrl: './lesson1-completed-toggle-form.component.html',
  styleUrls: ['./lesson1-completed-toggle-form.component.scss'],
  providers: [
    {
      provide: NG_VALUE_ACCESSOR,
      useExisting: Lesson1CompletedToggleFormComponent,
      multi: true
    }
  ]
})
export class Lesson1CompletedToggleFormComponent
  implements OnDestroy, ControlValueAccessor {
  control: FormControl;
  private _destroying = new Subject<void>();
  private _onTouched;

  writeValue(v: boolean) {
    if (!this.control) {
      this.control = new FormControl(v);
    } else {
      this.control.setValue(v);
    }
  }

  registerOnChange(fn) {
    this.control.valueChanges
      .pipe(
        takeUntil(this._destroying),
        tap(v => fn(v))
      )
      .subscribe(() => {}, () => {}, () => console.log('completed'));
  }

  registerOnTouched(fn) {
    this._onTouched = fn;
  }

  blur() {
    this._onTouched();
  }

  setDisabledState(disable: boolean) {
    disable ? this.control.disable() : this.control.enable();
  }

  ngOnDestroy() {
    this._destroying.next();
  }
}
```

#### Html Template
```
<label class="switch" (blur)="blur()">
  <input type="checkbox" [formControl]="control" />
  <span class="slider"></span>
</label>
```


#### Custom Date Control
```
import { Component, OnDestroy } from '@angular/core';
import {
  ControlValueAccessor,
  FormControl,
  FormGroup,
  NG_VALUE_ACCESSOR
} from '@angular/forms';
import { Subject } from 'rxjs';
import { startWith, takeUntil, tap, map } from 'rxjs/operators';

const padZero = (n: number): string => (n < 10 ? `0${n}` : `${n}`);
const timeString = (date: Date) =>
  `${padZero(date.getHours())}:${padZero(date.getMinutes())}:${padZero(
    date.getSeconds()
  )}`;
const dateString = (date: Date) =>
  `${date.getFullYear()}-${padZero(date.getMonth() + 1)}-${padZero(
    date.getDate()
  )}`;

@Component({
  selector: 'forms-course-lesson2-completed-date-time-picker',
  templateUrl: './lesson2-completed-date-time-picker.component.html',
  styleUrls: ['./lesson2-completed-date-time-picker.component.css'],
  providers: [
    {
      provide: NG_VALUE_ACCESSOR,
      useExisting: Lesson2CompletedDateTimePickerComponent,
      multi: true
    }
  ]
})
export class Lesson2CompletedDateTimePickerComponent
  implements OnDestroy, ControlValueAccessor {
  private _destroying$ = new Subject<void>();
  form: FormGroup;
  _onTouched;

  writeValue(date: Date) {
    if (!this.form) {
      this.form = new FormGroup({
        date: new FormControl(dateString(date)),
        time: new FormControl(timeString(date))
      });
    } else {
      this.form.setValue({
        date: dateString(date),
        time: timeString(date)
      });
    }
  }

  registerOnChange(fn: any): void {
    this.form.valueChanges
      .pipe(
        takeUntil(this._destroying$),
        startWith(this.form.value),
        map(({ date, time }) =>
          date && time ? new Date(`${date} ${time}`) : null
        ),
        tap(fn)
      )
      .subscribe();
  }

  registerOnTouched(fn) {
    this._onTouched = fn;
  }

  blur() {
    this._onTouched();
  }

  setDisabledState(isDisabled: boolean) {
    isDisabled ? this.form.disable() : this.form.enable();
  }

  ngOnDestroy() {
    this._destroying$.next();
  }
}
```

#### Html Template
```
<form [formGroup]="form" (blur)="blur()">
  <input type="date" formControlName="date" />
  <input type="time" formControlName="time" step="1" />
</form>
```

#### Reusable Form
```
import { Component, OnDestroy } from '@angular/core';
import {
  ControlValueAccessor,
  NG_VALUE_ACCESSOR,
  FormGroup,
  FormControl
} from '@angular/forms';
import { Subject } from 'rxjs';
import { takeUntil, tap } from 'rxjs/operators';

interface Hero {
  name: string;
  stats: {
    attack: number;
    defense: number;
    speed: number;
    health;
  };
}

const stats = ['attack', 'defense', 'speed', 'health'];

@Component({
  selector: 'forms-course-lesson3-completed-hero-form',
  templateUrl: './lesson3-completed-hero-form.component.html',
  styleUrls: ['./lesson3-completed-hero-form.component.css'],
  providers: [
    {
      provide: NG_VALUE_ACCESSOR,
      useExisting: Lesson3CompletedHeroFormComponent,
      multi: true
    }
  ]
})
export class Lesson3CompletedHeroFormComponent
  implements OnDestroy, ControlValueAccessor {
  private _destroying$ = new Subject<void>();
  form: FormGroup;
  private _onTouched;
  stats = stats;

  writeValue(hero: Hero) {
    if (!this.form) {
      this.form = new FormGroup({
        name: new FormControl(hero.name),
        stats: new FormGroup(
          this.stats.reduce(
            (acc, statName) => ({
              ...acc,
              [statName]: new FormControl(hero.stats[statName])
            }),
            {}
          )
        )
      });
    } else {
      this.form.setValue(hero);
    }
  }

  registerOnChange(fn) {
    this.form.valueChanges
      .pipe(
        takeUntil(this._destroying$),
        tap(fn)
      )
      .subscribe();
  }

  registerOnTouched(fn) {
    this._onTouched = fn;
  }

  blur() {
    this._onTouched();
  }

  setDisabledState(isDisabled: boolean) {
    isDisabled ? this.form.disable() : this.form.enable();
  }

  ngOnDestroy() {
    this._destroying$.next();
  }
}
```
#### Html Template
```
<form [formGroup]="form">
  <mat-card>
    <mat-card-title>
      <legend>Hero Form</legend>
    </mat-card-title>
    <mat-card-content>
      <mat-form-field appearance="outline">
        <mat-label>Hero Name</mat-label>
        <input matInput formControlName="name" />
      </mat-form-field>

      <form formGroupName="stats">
        <mat-card
          class="stat-card"
          [class.total-stats-error]="form['errors']?.totalStats"
        >
          <mat-card-title>
            <legend>Hero Stats</legend>
          </mat-card-title>

          <div *ngFor="let statName of stats">
            <mat-form-field appearance="outline">
              <mat-label>{{ statName | titlecase }}</mat-label>
              <input matInput type="number" [formControlName]="statName" />
            </mat-form-field>
          </div>
        </mat-card>
      </form>
    </mat-card-content>
  </mat-card>
</form>
```