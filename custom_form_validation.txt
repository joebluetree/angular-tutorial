## Custom Template Form Validation

#### Validation Diretive
```
import { Directive } from '@angular/core';
import { AbstractControl, NG_VALIDATORS, ValidationErrors, Validator } from '@angular/forms';

@Directive({
  selector: '[NumberValidator]',
  providers: [{ provide: NG_VALIDATORS, useClass: NumberValidatorDirective, multi: true }]
})
export class NumberValidatorDirective implements Validator {
  validate(control: AbstractControl<any, any>): ValidationErrors | null {
    if (!control.value) {
      return { 'ValueError': true };
    }
    else if (control.value && control.value < 0) {
      return { 'ValueError': true };
    }
    return null;
  }
}
```
