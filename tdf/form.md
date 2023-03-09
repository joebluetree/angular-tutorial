## Template Driven Form

#### Main Form
```
form #frm="ngForm"
#code="ngModel" name="code"
code.touched && code.invalid
code.errors?.['required']
[ngModelGroup]="group"
<fieldset>
<legend>

```

```
<form #frm="ngForm">
  <div class="container-fluid">
    <div class="row border mt-1">
      <div class="col-md-6 p-1">
        <fieldset>
          <legend>Basic Info</legend>
          <div class="form-group row">
            <label class="col-md-4 form-label form-label-sm" for="">Code</label>
            <div class="col-md-8">
              <input type="text" class="form-control form-control-sm" #code="ngModel" name="code"
                [(ngModel)]="contact.basic_data.code" required minlength="3">
              <div *ngIf="code.touched && code.invalid" class="text-danger">
                <span *ngIf="code.errors?.['required']">Code Required</span>
                <span *ngIf="code.errors?.['minlength']">Minimum 3 characters required </span>
              </div>
            </div>
          </div>
          <div class="form-group row">
            <label class="col-md-4 form-label form-label-sm" for="">Name</label>
            <div class="col-md-8">
              <input type="text" class="form-control form-control-sm" #name="ngModel" name="name"
                [(ngModel)]="contact.basic_data.name">
            </div>
          </div>
        </fieldset>
      </div>
    </div>
    <div class="row border  mt-1">
      <div class="col-md-6 p-1">
        <app-contact-address [group]="'Home_Address'" [address]="contact.home_address">Home
          Address</app-contact-address>
      </div>
      <div class="col-md-6 p-1">
        <app-contact-address [group]="'Contact_Address'" [address]="contact.contact_address"> Contact Address
        </app-contact-address>
      </div>
    </div>
  </div>
  <div class="border p-2 mt-1">
    <button class="btn btn-success btn-sm" [disabled]="frm.invalid">Save</button>
  </div>
</form>


<div class="border p-2 mt-1">
  <div class="form-check">
    <input type="checkbox" class="form-check-input" #chk="ngModel" ngModel>
    <label class="form-check-label" for="chk">Show Model</label>
  </div>
</div>
<div class="border mt-1" *ngIf="chk.value" (click)="show(chk.value)">
  Form Object
  <pre>{{frm.value | json}}</pre>
  Model Object
  <pre>{{contact | json}}</pre>
</div>
```

#### Sub Form
```
<div [ngModelGroup]="group">
  <div>
    <fieldset>
      <legend><ng-content></ng-content></legend>
      <div class="form-group row">
        <label class="col-md-4 form-label form-label-sm" for="">Address</label>
        <div class="col-md-8">
          <input type="text" class="form-control form-control-sm" #address="ngModel" name="address"
            [(ngModel)]="addressRec.address" required minlength="3">
          <div *ngIf="address.touched && address.invalid" class="text-danger">
            <span *ngIf="address.errors?.['required']">Code Required</span>
          </div>
        </div>
      </div>
      <div class="form-group row">
        <label class="col-md-4 form-label form-label-sm" for="">City</label>
        <div class="col-md-8">
          <input type="text" class="form-control form-control-sm" #city="ngModel" name="city"
            [(ngModel)]="addressRec.city">
        </div>
      </div>
    </fieldset>
  </div>
</div>
```
#### Component Provider
```
@Component({
  selector: 'app-contact-address',
  templateUrl: './contact-address.component.html',
  styleUrls: ['./contact-address.component.css'],
  viewProviders: [
    { provide: ControlContainer, useExisting: NgForm }
  ]
})
```