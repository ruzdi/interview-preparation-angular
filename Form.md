### 1. What is the difference between Template-driven and Reactive Forms?


1. Form Creation and Control

Template-driven Forms:

Form control is defined in the template (HTML) using Angular directives like ngModel, ngForm, and ngSubmit.
Angular automatically creates and manages the form controls behind the scenes based on the template structure.
This approach is declarative: the form structure is created directly in the HTML.

```
<form #userForm="ngForm">
  <input type="text" [(ngModel)]="user.name" name="name" required>
</form>
```

Reactive Forms:

Form control is defined in the component class using FormControl, FormGroup, or FormBuilder. The form is explicitly created and managed in the TypeScript class.

This approach is imperative: the form structure is created programmatically, providing more control over the form’s behavior.

```
this.userForm = new FormGroup({
  name: new FormControl('', Validators.required)
});
```

2. Form Validation

Template-driven Forms:

Validation is mostly declarative and defined in the template using built-in HTML5 validation attributes (e.g., required, minlength).
Custom validators can be implemented but are harder to integrate.
Asynchronous validation is more challenging to implement.

`<input type="text" name="name" [(ngModel)]="user.name" required minlength="3">`


Reactive Forms:

Validation is defined programmatically in the component class using Validators and can be applied at the control level, allowing fine-grained control over validation.
Supports both synchronous and asynchronous validation easily.

```
this.userForm = new FormGroup({
  name: new FormControl('', [Validators.required, Validators.minLength(3)])
});
```

3. Data Binding

Template-driven Forms:

Uses two-way data binding via the [(ngModel)] directive to bind data between the form and the model.

`<input [(ngModel)]="user.name" name="name">`

Reactive Forms:

Does not rely on two-way data binding. Instead, it uses explicit control over data flow using FormControl.value or FormGroup.value.
Changes to form controls must be tracked manually, providing more flexibility and control.

`console.log(this.userForm.value);  // Access form data explicitly`

4. Control and Flexibility

Template-driven Forms:

Less flexible and harder to maintain for large forms or dynamic form scenarios.
Automatically handles form control creation and management, which can be simpler for small forms but less customizable.

Use case: Suitable for simple forms where not much dynamic behavior or complex logic is required.

Reactive Forms:

Offers greater control over form creation and data flow.
Easier to manage dynamic forms (adding/removing form controls at runtime) and apply complex logic or validation.

Use case: Preferred for large, complex forms that require dynamic behavior, custom validation, or fine-grained control.

5. Scalability and Structure

Template-driven Forms:

Harder to scale for large forms or complex scenarios since most of the form logic resides in the template.
Less structured, which can make it difficult to maintain or test forms in large applications.

Reactive Forms:

Much easier to scale as form logic is entirely separated into the component class, making the form structure more maintainable and testable.
More suited for forms with complex requirements (e.g., conditional form fields, complex validation rules).

6. Testing

Template-driven Forms:

Testing is more difficult since much of the form logic resides in the template and Angular internally manages the form controls.

Example: Harder to mock user inputs and form submission due to the reliance on the template.

Reactive Forms:

Easier to unit test because the form model is available in the component class, and you can directly manipulate and test form controls.

Example: You can programmatically set values in form controls and check the validity of the form in tests.

7. Error Handling

Template-driven Forms:

Error handling is done via the template, which can get cluttered when handling many different validation states.

```
<div *ngIf="userForm.form.controls.name?.invalid && userForm.form.controls.name?.touched">
  Name is required
</div>
```

Reactive Forms:

Error handling can be centralized in the component class, which provides cleaner templates.

```
get nameErrors() {
  const nameControl = this.userForm.get('name');
  if (nameControl.hasError('required')) return 'Name is required';
  if (nameControl.hasError('minlength')) return 'Name must be at least 3 characters';
}
```

8. Dynamic Forms

Template-driven Forms:

Adding or removing form controls dynamically is harder and less intuitive.

Reactive Forms:

Highly flexible for dynamic forms, allowing the addition or removal of form controls at runtime using methods like addControl() and removeControl().

`this.userForm.addControl('address', new FormControl(''));`


### 2. Example of Template-driven Form & Reactive Form

1. Template-driven Form Example


```
// app.module.ts 

import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { FormsModule } from '@angular/forms';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, FormsModule], // Import FormsModule
  bootstrap: [AppComponent]
})
export class AppModule {}

```

Template-driven Form (HTML)

```
<h2>Template-driven Form</h2>

<form #userForm="ngForm" (ngSubmit)="onSubmit(userForm)">
  <!-- Input Field with Validation -->
  <div>
    <label for="name">Name:</label>
    <input id="name" name="name" [(ngModel)]="user.name" required minlength="3" />
    <div *ngIf="userForm.controls.name?.invalid && userForm.controls.name?.touched">
      <small *ngIf="userForm.controls.name.errors?.required">Name is required</small>
      <small *ngIf="userForm.controls.name.errors?.minlength">Name must be at least 3 characters</small>
    </div>
  </div>

  <!-- Email Field with Validation -->
  <div>
    <label for="email">Email:</label>
    <input id="email" name="email" [(ngModel)]="user.email" required email />
    <div *ngIf="userForm.controls.email?.invalid && userForm.controls.email?.touched">
      <small *ngIf="userForm.controls.email.errors?.required">Email is required</small>
      <small *ngIf="userForm.controls.email.errors?.email">Email must be valid</small>
    </div>
  </div>

  <!-- Password Field with Validation -->
  <div>
    <label for="password">Password:</label>
    <input id="password" name="password" type="password" [(ngModel)]="user.password" required minlength="6" />
    <div *ngIf="userForm.controls.password?.invalid && userForm.controls.password?.touched">
      <small *ngIf="userForm.controls.password.errors?.required">Password is required</small>
      <small *ngIf="userForm.controls.password.errors?.minlength">Password must be at least 6 characters</small>
    </div>
  </div>

  <!-- Confirm Password Field -->
  <div>
    <label for="confirmPassword">Confirm Password:</label>
    <input id="confirmPassword" name="confirmPassword" type="password" [(ngModel)]="user.confirmPassword" required />
    <div *ngIf="userForm.controls.confirmPassword?.touched && user.confirmPassword !== user.password">
      <small>Passwords must match</small>
    </div>
  </div>


  <!-- Radio Buttons -->
  <div>
    <label>Gender:</label>
    <input type="radio" name="gender" [(ngModel)]="user.gender" value="male" required /> Male
    <input type="radio" name="gender" [(ngModel)]="user.gender" value="female" required /> Female
    <div *ngIf="userForm.controls.gender?.invalid && userForm.controls.gender?.touched">
      <small>Gender is required</small>
    </div>
  </div>

  <!-- Checkbox -->
  <div>
    <label>
      <input type="checkbox" name="acceptTerms" [(ngModel)]="user.acceptTerms" required />
      Accept Terms & Conditions
    </label>
    <div *ngIf="userForm.controls.acceptTerms?.invalid && userForm.controls.acceptTerms?.touched">
      <small>You must accept the terms</small>
    </div>
  </div>

  <!-- Select Dropdown -->
  <div>
    <label for="country">Country:</label>
    <select id="country" name="country" [(ngModel)]="user.country" required>
      <option value="" disabled>Select your country</option>
      <option *ngFor="let country of countries" [value]="country">{{ country }}</option>
    </select>
    <div *ngIf="userForm.controls.country?.invalid && userForm.controls.country?.touched">
      <small>Country is required</small>
    </div>
  </div>

  <!-- Multi-select Checkboxes -->
  <div>
    <label>Languages:</label>
    <div *ngFor="let language of languages">
      <label>
        <input type="checkbox" name="languages" [(ngModel)]="user.languages" [value]="language" />
        {{ language }}
      </label>
    </div>
  </div>

  <button type="submit" [disabled]="userForm.invalid || user.password !== user.confirmPassword">Submit</button>
</form>

<div *ngIf="submitted">
  <h3>Form Submitted Successfully!</h3>
  <pre>{{ user | json }}</pre>
</div>

```

Component Logic for Template-driven Form


```
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html'
})
export class AppComponent {
  user = {
    name: '',
    email: '',
    password: '',
    gender: '',
    acceptTerms: false,
    country: '',
    languages: []
  };

  countries = ['USA', 'Canada', 'UK', 'Germany', 'India'];
  languages = ['English', 'Spanish', 'French', 'German', 'Chinese'];

  submitted = false;

  onSubmit(form: any) {
    if (form.valid && this.user.password === this.user.confirmPassword) {
      this.submitted = true;
      console.log('Form Submitted!', this.user);
    }
  }
}
```

2. Reactive Form Example

```
<h2>Reactive Form with Confirm Password</h2>

<form [formGroup]="userForm" (ngSubmit)="onSubmit()">
  <!-- Name Field -->
  <div>
    <label for="name">Name:</label>
    <input id="name" formControlName="name" />
    <div *ngIf="userForm.get('name')?.invalid && userForm.get('name')?.touched">
      <small *ngIf="userForm.get('name')?.errors?.required">Name is required</small>
      <small *ngIf="userForm.get('name')?.errors?.minlength">Name must be at least 3 characters</small>
    </div>
  </div>

  <!-- Email Field -->
  <div>
    <label for="email">Email:</label>
    <input id="email" formControlName="email" />
    <div *ngIf="userForm.get('email')?.invalid && userForm.get('email')?.touched">
      <small *ngIf="userForm.get('email')?.errors?.required">Email is required</small>
      <small *ngIf="userForm.get('email')?.errors?.email">Email must be valid</small>
    </div>
  </div>

  <!-- Password Field -->
  <div>
    <label for="password">Password:</label>
    <input id="password" formControlName="password" type="password" />
    <div *ngIf="userForm.get('password')?.invalid && userForm.get('password')?.touched">
      <small *ngIf="userForm.get('password')?.errors?.required">Password is required</small>
      <small *ngIf="userForm.get('password')?.errors?.minlength">Password must be at least 6 characters</small>
    </div>
  </div>

  <!-- Confirm Password Field -->
  <div>
    <label for="confirmPassword">Confirm Password:</label>
    <input id="confirmPassword" formControlName="confirmPassword" type="password" />
    <div *ngIf="userForm.hasError('passwordMismatch') && userForm.get('confirmPassword')?.touched">
      <small>Passwords must match</small>
    </div>
  </div>

  <!-- Gender (Radio Buttons) -->
  <div>
    <label>Gender:</label>
    <input type="radio" formControlName="gender" value="male" /> Male
    <input type="radio" formControlName="gender" value="female" /> Female
    <div *ngIf="userForm.get('gender')?.invalid && userForm.get('gender')?.touched">
      <small>Gender is required</small>
    </div>
  </div>

  <!-- Checkbox (Terms & Conditions) -->
  <div>
    <label>
      <input type="checkbox" formControlName="acceptTerms" />
      Accept Terms & Conditions
    </label>
    <div *ngIf="userForm.get('acceptTerms')?.invalid && userForm.get('acceptTerms')?.touched">
      <small>You must accept the terms</small>
    </div>
  </div>

  <!-- Select Dropdown for Country -->
  <div>
    <label for="country">Country:</label>
    <select id="country" formControlName="country">
      <option value="" disabled>Select your country</option>
      <option *ngFor="let country of countries" [value]="country">{{ country }}</option>
    </select>
    <div *ngIf="userForm.get('country')?.invalid && userForm.get('country')?.touched">
      <small>Country is required</small>
    </div>
  </div>

  <!-- Multi-select Checkboxes for Languages -->
  <div>
    <label>Languages:</label>
    <div *ngFor="let language of languages">
      <label>
        <input type="checkbox" formControlName="languages" [value]="language" />
        {{ language }}
      </label>
    </div>
  </div>

  <button type="submit" [disabled]="userForm.invalid">Submit</button>
</form>

<div *ngIf="submitted">
  <h3>Form Submitted Successfully!</h3>
  <pre>{{ userForm.value | json }}</pre>
</div>


```
Component Logic for Reactive Form

```
import { Component, OnInit } from '@angular/core';
import { FormGroup, FormBuilder, Validators, AbstractControl } from '@angular/forms';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html'
})
export class AppComponent implements OnInit {
  userForm!: FormGroup;
  submitted = false;

  countries = ['USA', 'Canada', 'UK', 'Germany', 'India'];
  languages = ['English', 'Spanish', 'French', 'German', 'Chinese'];

  constructor(private fb: FormBuilder) {}

  ngOnInit() {
    // Initialize the form and add validations
    this.userForm = this.fb.group({
      name: ['', [Validators.required, Validators.minLength(3)]],
      email: ['', [Validators.required, Validators.email]],
      password: ['', [Validators.required, Validators.minLength(6)]],
      confirmPassword: ['', Validators.required],
      gender: ['', Validators.required],
      acceptTerms: [false, Validators.requiredTrue],
      country: ['', Validators.required],
      languages: [[]],  // Multi-select checkboxes
    }, { validators: this.passwordMatchValidator });
  }

  // Custom validator to check if password and confirmPassword fields match
  passwordMatchValidator(form: AbstractControl): { [key: string]: boolean } | null {
    const password = form.get('password');
    const confirmPassword = form.get('confirmPassword');
    if (password?.value !== confirmPassword?.value) {
      return { passwordMismatch: true };
    }
    return null;
  }

  onSubmit() {
    if (this.userForm.valid) {
      this.submitted = true;
      console.log('Form Submitted!', this.userForm.value);
    }
  }
}
```

### 3. How do you create custom validators in Angular?

```
import { AbstractControl, ValidationErrors, ValidatorFn } from '@angular/forms';
import { Observable, of } from 'rxjs';
import { delay, map } from 'rxjs/operators';

/**
 * Custom validator to check if the name is forbidden.
 * @param forbiddenNames An array of forbidden names.
 * @returns ValidatorFn A function that performs the validation.
 */
export function forbiddenNameValidator(forbiddenNames: string[]): ValidatorFn {
  return (control: AbstractControl): ValidationErrors | null => {
    const forbidden = forbiddenNames.includes(control.value);
    return forbidden ? { forbiddenName: { value: control.value } } : null;
  };
}

/**
 * Asynchronous validator to check if the username is already taken.
 * @returns A function that returns an observable to simulate an API call.
 */
export function usernameTakenValidator(): (control: AbstractControl) => Observable<ValidationErrors | null> {
  return (control: AbstractControl): Observable<ValidationErrors | null> => {
    const takenUsernames = ['admin', 'user', 'test'];
    return of(takenUsernames.includes(control.value)).pipe(
      delay(1000), // Simulating an API delay
      map(isTaken => (isTaken ? { usernameTaken: true } : null))
    );
  };
}

```

Component: 
```
import { Component, OnInit } from '@angular/core';
import { FormGroup, FormBuilder, Validators } from '@angular/forms';
import { forbiddenNameValidator, usernameTakenValidator } from './custom-validators';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html'
})
export class AppComponent implements OnInit {
  userForm!: FormGroup;

  constructor(private fb: FormBuilder) {}

  ngOnInit() {
    this.userForm = this.fb.group({
      // Passing an array to the forbiddenNameValidator
      username: ['', [Validators.required, forbiddenNameValidator(['admin', 'superadmin'])], [usernameTakenValidator()]], // Synchronous + Asynchronous Validators
      email: ['', [Validators.required, Validators.email]],
    });
  }

  onSubmit() {
    if (this.userForm.valid) {
      console.log('Form Submitted', this.userForm.value);
    }
  }
}

```
HTML Template

```
<form [formGroup]="userForm" (ngSubmit)="onSubmit()">
  <!-- Username Field -->
  <div>
    <label for="username">Username:</label>
    <input id="username" formControlName="username" />
    <div *ngIf="userForm.get('username')?.invalid && userForm.get('username')?.touched">
      <small *ngIf="userForm.get('username')?.errors?.required">Username is required</small>
      <small *ngIf="userForm.get('username')?.errors?.forbiddenName">Username cannot be 'admin' or 'superadmin'</small>
      <small *ngIf="userForm.get('username')?.errors?.usernameTaken">This username is already taken</small>
    </div>
  </div>

  <!-- Email Field -->
  <div>
    <label for="email">Email:</label>
    <input id="email" formControlName="email" />
    <div *ngIf="userForm.get('email')?.invalid && userForm.get('email')?.touched">
      <small *ngIf="userForm.get('email')?.errors?.required">Email is required</small>
      <small *ngIf="userForm.get('email')?.errors?.email">Email must be valid</small>
    </div>
  </div>

  <button type="submit" [disabled]="userForm.invalid">Submit</button>
</form>

```