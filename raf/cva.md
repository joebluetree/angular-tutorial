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
