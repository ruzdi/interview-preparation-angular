# Angular top interview question

### 1. What is Angular?

Angular is a popular open-source web application framework written in TypeScript, which brings structure and consistency, it is used for building dynamic, single-page web applications (SPAs) and allows developers to create rich and responsive web interfaces with a strong focus on maintainability and performance.

It uses HTML syntax to express your application's components clearly.


### 2. What are the main features of Angular?


Key features of Angular include:

1. Component-Based Architecture: Angular applications are built using a component-based approach. Components encapsulate the view, logic, and styles for a specific part of the user interface, making it easier to manage and scale large applications.

2. Two-Way Data Binding: Angular automatically synchronizes data between the model (data source) and the view (UI), making it easier to develop dynamic user interfaces.

3. Dependency Injection: Angular provides a powerful dependency injection system, which makes it easy to manage and inject services or dependencies into different parts of the application.

4. Directives: Angular directives are special markers on a DOM element that tell Angular to attach specific behavior to that element or modify its appearance. This allows developers to create reusable UI components.

5. Routing: Angular has a built-in routing module that allows developers to create a seamless navigation experience within single-page applications, without needing to reload the entire page.

6. TypeScript: Angular is built using TypeScript, a statically typed superset of JavaScript that provides features such as type checking and better tooling for large-scale applications.

7. Reactive Forms and Template-Driven Forms: Angular provides two ways to handle forms—reactive forms for more complex scenarios and template-driven forms for simpler use cases.

8. RxJS for Reactive Programming: Angular uses RxJS (Reactive Extensions for JavaScript) for handling asynchronous events like HTTP requests, making it easier to work with streams of data.


### 3. What is a component in Angular?

A component is a fundamental building block of Angular applications. It consists of a TypeScript class, an HTML template, and CSS/SCSS styles.

Example: Creating a reusable header component for your application.


```
import { Component } from '@angular/core';

@Component({
  selector: 'app-example',  // Custom HTML tag to use the component
  templateUrl: './example.component.html',
  styleUrls: ['./example.component.css']
})
export class ExampleComponent {
  title = 'Hello, Angular!';

  onClick() {
    this.title = 'Button Clicked!';
  }
}
```

example.component.html
```
<div>
  <h1>{{ title }}</h1>
  <button (click)="onClick()">Click me</button>
</div>
```

example.component.css
```
h1 {
  color: blue;
}
```

### 4. What is the use of @Component decorator?

@Component decorator defines metadata about the component. 

```
@Component({
  selector: 'app-example', // 
  templateUrl: './example.component.html',
  styleUrls: ['./example.component.css'],
  providers: [SomeService],
  encapsulation: ViewEncapsulation.Emulated
})
```
#### selector: 
Selector defines the HTML tag that will be used to embed the component within the application’s HTML template.

##### template & templateUrl: 
The template (or templateUrl) defines HTML layout that this component renders.

##### styles & styleUrls:
styles (or templateUrl) defines the CSS or SCSS styles applied to this component. 

#### providers: 
The providers array can be used to define services or other dependencies that are specific to this component. This allows the component to have its own instance of a service, separate from other parts of the application.
* see services section for more details and type of providers.

#### encapsulation:
The encapsulation property controls how styles are applied to the component. Angular provides three types of view encapsulation:

#### animation
Define animations using the animations property.

### 5. What are different types of selectors?

Different Types of Selctors:

1. Element Selector:
This is the most common type of selector, where the component is used as a custom HTML element (tag)

```
@Component({
  selector: 'app-example',
  template: '<p>Element Selector</p>',
})
export class ExampleComponent {}
```

Uses:
`<app-example></app-example>`


2. Attribute Selector:
```
@Component({
  selector: '[app-example]',
  template: '<p>Attribute Selector</p>',
})
export class ExampleComponent {}
```

Uses: 
`<div app-example></div>`

3. Class Selector:

```
@Component({
  selector: '.app-example',
  template: '<p>Class Selector</p>',
})
export class ExampleComponent {}
```
Uses:
`<div class="app-example"></div>`

4. Complex Selector (Combining Types):
```
@Component({
  selector: 'button[app-example]',
  template: '<p>Combined Selector</p>',
})
export class ExampleComponent {}
```
Uses
`<button app-example>Click me</button>`


### 6. What are different types of view encapsulation?

The encapsulation property controls how styles are applied to the component. Angular provides three types of view encapsulation:

1. ViewEncapsulation.Emulated (default): Styles are scoped to the component using shadow DOM-like behavior, but without actual shadow DOM.
2. ViewEncapsulation.ShadowDom: Uses real shadow DOM, meaning styles are encapsulated and won't affect other parts of the page.
3. ViewEncapsulation.None: No style encapsulation, meaning styles can affect the entire application.


### 6. What is service?

Services are classes that contain business logic, data access, or other reusable functionalities that can be shared across components, directives, and other parts of the application. Services are typically injected into components or other services using Angular's dependency injection system.

```
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root',  // Makes the service available globally in the app
})
export class MyService {

  constructor() { }

  // Example method
  getData(): string {
    return 'This is service data!';
  }
}
```

After service is created, you can to make it available to be injected. There are two main ways to provide a service:

Provided in root: The service is available application-wide (global scope) by setting the providedIn: 'root' option.

Module/Component Providers: You can provide the service at the module or component level if you want to scope it to a specific part of the app.

### 7. What is provider and what are the different types of providers?

A provider is a way to register a service (or any dependency) with Angular’s dependency injection system.

Providers tell Angular how to create or deliver an instance of the service or dependency.

Without a provider, Angular wouldn't know how to supply an instance of the service when it's requested in a component, directive, or other service.


1. Class Provider:
This is the most common type of provider. It tells Angular to create an instance of a service class when it is injected.

```
@Injectable()
export class MyService {
  // Service logic here
}

@Component({
  selector: 'app-example',
  templateUrl: './example.component.html',
  providers: [MyService],  // Class provider
})
export class ExampleComponent {
  constructor(private myService: MyService) {}
}
```

2. Value Provider:
A value provider allows you to provide a simple value, object, or configuration instead of a class instance. It is typically used when you need to provide a static value (e.g., configuration settings).

```
const MY_VALUE = 'Hello, Angular!';

@Component({
  selector: 'app-example',
  templateUrl: './example.component.html',
  providers: [{ provide: 'MyValue', useValue: MY_VALUE }],
})
export class ExampleComponent {
  constructor(@Inject('MyValue') private value: string) {
    console.log(value); // Outputs: Hello, Angular!
  }
}
```

3. Factory Provider:
Factory providers are used when you need to define a custom logic to create the service instance. This is helpful when the instance creation depends on specific conditions or external data.

```
export function myServiceFactory(): MyService {
  return new MyService();
}

@Component({
  selector: 'app-example',
  templateUrl: './example.component.html',
  providers: [{ provide: MyService, useFactory: myServiceFactory }],
})
export class ExampleComponent {
  constructor(private myService: MyService) {}
}
```

4. Existing Provider:

This provider allows you to alias one token (or service) to another. This is useful if you want to reuse an existing service under a different name.

```
@Injectable()
export class NewService {}

@Component({
  selector: 'app-example',
  templateUrl: './example.component.html',
  providers: [
    { provide: 'OldService', useExisting: NewService }
  ],
})
export class ExampleComponent {
  constructor(@Inject('OldService') private oldService: NewService) {}
}
```

5. Use Class Provider:

This provider type allows you to use one class when another class is requested. This can be helpful when a service needs to extend or replace an existing service.


```
@Injectable()
export class NewService {}

@Injectable()
export class OldService extends NewService {}

@Component({
  selector: 'app-example',
  templateUrl: './example.component.html',
  providers: [{ provide: OldService, useClass: NewService }],
})
export class ExampleComponent {
  constructor(private oldService: OldService) {}
}


```

6. Multi Providers:

Multi providers allow you to provide multiple values for the same token. It is often used when you want to provide a collection of items (e.g., multiple interceptors or validators).


```
@Injectable()
export class FirstService {}

@Injectable()
export class SecondService {}

@Component({
  selector: 'app-example',
  templateUrl: './example.component.html',
  providers: [
    { provide: 'MultiToken', useClass: FirstService, multi: true },
    { provide: 'MultiToken', useClass: SecondService, multi: true }
  ],
})
export class ExampleComponent {
  constructor(@Inject('MultiToken') private services: any[]) {
    console.log(services); // Outputs: [FirstService instance, SecondService instance]
  }
}
```

7. Provided in Root (Service Scoping):

In modern Angular, services are often provided at the root level using the providedIn property in the @Injectable decorator, instead of manually configuring providers in providers arrays. This makes the service available application-wide and ensures tree-shaking for unused services.

```
@Injectable({
  providedIn: 'root'  // Available globally
})
export class MyService {}
```


### 8. How can we create Singleton services?
Singleton services are services that have only one instance created for the entire lifetime of the application.

1. Using providedIn: 'root'

```
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'  // The service will be a singleton and available throughout the app
})
export class SingletonService {
  private counter = 0;

  increment() {
    this.counter++;
  }

  getCounter() {
    return this.counter;
  }
}
```

2. Providing the Service in a Module

We can provide a service at the module level to make it a singleton for all components in that module.

```
// singleton.service.ts
import { Injectable } from '@angular/core';

@Injectable()
export class SingletonService {
  private counter = 0;

  increment() {
    this.counter++;
  }

  getCounter() {
    return this.counter;
  }
}

// app.module.ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent } from './app.component';
import { SingletonService } from './singleton.service';

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule],
  providers: [SingletonService],  // The service is provided here, making it a singleton
  bootstrap: [AppComponent]
})
export class AppModule {}
```


### 9. How can we create non-singleton services?


1. Provide the Service at the Component Level:

```
import { Injectable } from '@angular/core';

@Injectable()
export class NonSingletonService {
  private counter = 0;

  increment() {
    this.counter++;
  }

  getCounter() {
    return this.counter;
  }
}
````

Uses: 

```
import { Component } from '@angular/core';
import { NonSingletonService } from './non-singleton.service';

@Component({
  selector: 'app-example',
  template: `
    <p>Counter: {{ counter }}</p>
    <button (click)="increment()">Increment</button>
  `,
  providers: [NonSingletonService]  // New instance of NonSingletonService for each component instance
})
export class ExampleComponent {
  counter: number;

  constructor(private nonSingletonService: NonSingletonService) {
    this.counter = this.nonSingletonService.getCounter();
  }

  increment() {
    this.nonSingletonService.increment();
    this.counter = this.nonSingletonService.getCounter();
  }
}

```

2. Provide the Service in a Specific Module:

We can create non-singleton services is by providing the service in a specific module (not in the root or AppModule). Each time a module is loaded, a new instance of the service will be created. This approach works well for feature modules or lazy-loaded modules.

```
import { Injectable } from '@angular/core';

@Injectable()
export class NonSingletonService {
  private counter = 0;

  increment() {
    this.counter++;
  }

  getCounter() {
    return this.counter;
  }
}
```
Provide the Service in a Module:

```
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FeatureComponent } from './feature.component';
import { NonSingletonService } from './non-singleton.service';

@NgModule({
  declarations: [FeatureComponent],
  imports: [CommonModule],
  providers: [NonSingletonService]  // Non-singleton, new instance for this module
})
export class FeatureModule {}

```

Using the Service in a Component within the Module:


```
import { Component } from '@angular/core';
import { NonSingletonService } from './non-singleton.service';

@Component({
  selector: 'app-feature',
  template: `
    <p>Counter: {{ counter }}</p>
    <button (click)="increment()">Increment</button>
  `
})
export class FeatureComponent {
  counter: number;

  constructor(private nonSingletonService: NonSingletonService) {
    this.counter = this.nonSingletonService.getCounter();
  }

  increment() {
    this.nonSingletonService.increment();
    this.counter = this.nonSingletonService.getCounter();
  }
}

```

*** Services provided in lazy-loaded modules are non-singleton, as new instances are created each time the module is loaded.



### 10. What is a directive in Angular? 


Directives are special markers on a DOM element that tell Angular to do something to that DOM element or its children. Directives can modify the DOM, change the appearance of elements, handle events, or extend the functionality of existing HTML elements.


### 11. What are different types of directive in angular?


There are three types of directives in Angular:

1. Component Directives: Technically, components are a special type of directive, with an associated template. Components are the most commonly used directives in Angular.

```
@Component({
  selector: 'app-example',
  template: '<h1>Hello from Component!</h1>'
})
export class ExampleComponent {
  // Component logic
}
```

2. Structural Directives: These directives alter the DOM structure by adding, removing, or manipulating elements in the DOM. They usually start with an asterisk (*) in the template and are applied to the host element.

Common Structural Directives:
* *ngIf: Conditionally add or remove elements from the DOM.
```
<div *ngIf="isVisible">This element is visible if isVisible is true.</div>
```

* *ngFor: Repeat a template for each item in a list.
```
<ul>
  <li *ngFor="let item of items">{{ item }}</li>
</ul>
```

* *ngSwitch: Switch between multiple elements based on a condition.

```
// app.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html'
})
export class AppComponent {
  selectedColor: string = 'red';
}
```

Html Template: 
```
<!-- app.component.html -->
<div [ngSwitch]="selectedColor">
  <p *ngSwitchCase="'red'">The color is Red!</p>
  <p *ngSwitchCase="'blue'">The color is Blue!</p>
  <p *ngSwitchCase="'green'">The color is Green!</p>
  <p *ngSwitchDefault>Color not found!</p>
</div>

<button (click)="selectedColor = 'red'">Red</button>
<button (click)="selectedColor = 'blue'">Blue</button>
<button (click)="selectedColor = 'green'">Green</button>
<button (click)="selectedColor = 'yellow'">Yellow</button>

```


Custom Structural Directive:

```
import { Directive, TemplateRef, ViewContainerRef } from '@angular/core';

@Directive({
  selector: '[appShow]'
})
export class ShowDirective {
  constructor(private templateRef: TemplateRef<any>, private viewContainer: ViewContainerRef) {}

  set appShow(condition: boolean) {
    if (condition) {
      this.viewContainer.createEmbeddedView(this.templateRef);
    } else {
      this.viewContainer.clear();
    }
  }
}
```

You would use this custom directive like this:

```
<div *appShow="true">This element will be displayed.</div>
```


3. Attribute Directives: These directives change the appearance or behavior of an existing element.

Common Attribute Directives:
ngClass: Dynamically add or remove CSS classes.
ngStyle: Dynamically apply inline styles.

Example of ngClass:

```
<div [ngClass]="{'active': isActive, 'inactive': !isActive}">
  This element will have the active or inactive class based on the condition.
</div>
```

Example of ngStyle:

```
<div [ngStyle]="{'color': textColor}">
  This text will have dynamic color.
</div>
```


Custom Attribute Directive:

```
import { Directive, ElementRef, Renderer2, HostListener } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  constructor(private el: ElementRef, private renderer: Renderer2) {}

  @HostListener('mouseenter') onMouseEnter() {
    this.renderer.setStyle(this.el.nativeElement, 'backgroundColor', 'yellow');
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.renderer.removeStyle(this.el.nativeElement, 'backgroundColor');
  }
}
```

Uses:

```
<p appHighlight>Hover over this text to see the effect of the directive.</p>
```


### 12. what is the difference between directive and component?

| **Feature**   | **Component**                                             | **Directive**                                                       |
|---------------|-----------------------------------------------------------|---------------------------------------------------------------------|
| **Purpose**   | Builds reusable blocks of the UI (view + logic).           | Adds behavior or modifies the DOM without creating a new view.       |
| **Template**  | Has its own template (HTML).                               | Does not have its own template, modifies the existing one.           |
| **Usage**     | Declared as a custom HTML element (e.g., `<app-example>`). | Applied as an attribute or modifies existing HTML elements.          |
| **Types**     | There’s only one type of component.                        | Two main types: structural and attribute directives.                 |
| **Example**   | `<app-example></app-example>`                              | `<div appHighlight></div>` or `*ngIf`, `*ngFor`.                     |


### 13. what is the Pipe?

A pipe is a feature used to transform data within a template. Pipes are used to format and display data in a specific way without modifying the original data. They work by taking in input data, processing it, and then returning the transformed output. Pipes are often used to format dates, numbers, currency, strings, or to filter data in lists.

Pipes are easy to use and can be chained to combine multiple transformations. Angular also provides both built-in pipes (such as date, currency, uppercase, etc.) and the ability to create custom pipes to handle specific transformation needs.

```
<p>{{ birthday | date:'longDate' }}</p> <!-- Transforms birthday to long date format -->
<p>{{ price | currency:'USD' }}</p> <!-- Formats price as USD currency -->
<p>{{ name | uppercase }}</p> <!-- Converts the name to uppercase -->
```

### 14. what are different types of Pipe?

1. Built-in Pipes:

Angular provides a wide variety of built-in pipes that handle common data transformations. Below are some of the most commonly used pipes:

date: Formats date values.
Example: {{ birthday | date:'longDate' }}
Output: May 15, 1990

currency: Formats a number as a currency.
Example: {{ price | currency:'USD':'symbol' }}
Output: $1,234.57

uppercase / lowercase: Converts text to uppercase or lowercase.
Example: {{ name | uppercase }}
Output: ANGULAR PIPES

number: Formats a number with decimal places.
Example: {{ 3.14159 | number:'1.2-2' }}
percent: Formats a number as a percentage.
Output: 3.14

Example: {{ 0.25 | percent }}
json: Converts an object or array to a JSON string.
Output: 25%
Example: {{ object | json }}
Output:
```
{
  "name": "John",
  "age": 30,
  "city": "New York"
}
```

slice: Extracts a portion of an array or string.
Example:
<p>{{ description | slice:0:10 }}...</p>
Output:
This is a...

Example 2:
<ul>
  <li *ngFor="let item of items | slice:1:3">{{ item }}</li>
</ul>

Input:
items = ['Apple', 'Banana', 'Cherry', 'Date', 'Elderberry'];

Output: 
Banana
Cherry


keyvalue: Converts an object into an array of key-value pairs.
Input:
items = ['Apple', 'Banana', 'Cherry', 'Date', 'Elderberry'];

Output: 
Banana
Cherry


keyvalue: Converts an object into an array of key-value pairs.
Example: 
```
<ul>
  <li *ngFor="let entry of person | keyvalue">
    {{ entry.key }}: {{ entry.value }}
  </li>
</ul>
```

Input:
person = { name: 'John', age: 30, city: 'New York' };


Output: 
Copy code
name: John
age: 30
city: New York


2. Example of a Custom Pipe:

```
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'reverseString'
})
export class ReverseStringPipe implements PipeTransform {
  transform(value: string): string {
    return value.split('').reverse().join('');
  }
}
```

Use the Custom Pipe in the Template:

`<p>{{ 'Angular' | reverseString }}</p>` <!-- Outputs: ralugnA -->


3. Pure vs. Impure Pipes:

Pure pipes are only recalculated when their inputs change (default behavior).

```
@Pipe({
  name: 'myPurePipe',
  pure: true  // This is the default behavior
})
export class MyPurePipe implements PipeTransform {
  transform(value: any): any {
    return someTransformation(value);
  }
}
```

Impure pipes are recalculated every time Angular’s change detection runs, which can be useful for certain scenarios like working with mutable data but comes with a performance cost.


```
@Pipe({
  name: 'myImpurePipe',
  pure: false  // This marks the pipe as impure
})
export class MyImpurePipe implements PipeTransform {
  transform(value: any): any {
    return someDynamicTransformation(value);
  }
}
```




### 15. Explain two-way data binding in Angular.
two-way data binding is a powerful feature that allows synchronization of data between the model (the data in your component) and the view (the template or UI). This means that any changes made in the view (for example, through user input) are immediately reflected in the model, and vice versa — any changes in the model are immediately updated in the view. 

Angular uses [(ngModel)] as the syntax for two-way data binding, where:

[] represents property binding (data flowing from the model to the view).
() represents event binding (data flowing from the view back to the model).

Example: 

Comonent:
```
import { Component } from '@angular/core';

@Component({
  selector: 'app-example',
  templateUrl: './example.component.html'
})
export class ExampleComponent {
  username: string = 'John Doe'; // Model
}

```

Template: 
```
<input [(ngModel)]="username" placeholder="Enter your name">
<p>Your name is: {{ username }}</p>
```



### 16. Differences between one-way binding and two-way binding
one-way binding and two-way binding are techniques used to bind data between the component (model) and the template (view).

#### Types of One-Way Binding:

1. From Model to View (Property Binding):

* Data flows from the component (model) to the view (template).
* Any changes in the model automatically update the view.
* However, changes in the view do not affect the model.

Example: 
Interpolation: {{ expression }}
Property binding: [property]="expression"

2. From View to Model (Event Binding):

* Data flows from the view (template) to the component (model) when an event (like a button click or input change) occurs.
* Any change in the view updates the model based on an event handler, but the model does not automatically reflect in the view unless specifically coded to do so.


`<input type="text" (input)="username = $event.target.value">`


#### Two-Way Data Binding:

Two-way data binding refers to data flowing in both directions between the model and the view.
1. From Model to View: Data is initially bound from the model (component) to the view (template).
2. From View to Model: When the user interacts with the view (e.g., input fields), the changes are propagated back to the model automatically.

```
<input [(ngModel)]="username" placeholder="Enter your name">
<p>{{ username }}</p>
```

### 17. What is the digest cycle in Angular?

In modern Angular (versions 2 and above), the concept of the digest cycle no longer exists. The digest cycle was a core mechanism used in AngularJS (version 1.x) for change detection, but it has been replaced by a more efficient mechanism called the zone-based change detection in modern Angular.

### 18. What are the Key Features of Change Detection in Modern Angular:

Key Features of Change Detection in Modern Angular:

1. Zone.js:

Zone.js is a library used by Angular to automatically detect changes in application.

It intercepts asynchronous operations (e.g., user events, HTTP requests, setTimeout, etc.) and triggers Angular’s change detection process whenever those operations complete.

By using zones, Angular knows when to check for changes in the component’s data and update the view accordingly, without the developer needing to manually manage the cycle.


2. Change Detection Mechanism:

Angular's change detection mechanism uses ViewModel and component trees to check for changes. When Angular detects a change, it propagates the change down the component tree and updates the necessary parts of the DOM.

3. OnPush Change Detection Strategy:

Angular allows for more control over the change detection mechanism through the OnPush change detection strategy. With OnPush, change detection is triggered only when:
* The component's input properties change.
* An event or asynchronous operation happens inside the component.
* This strategy helps optimize performance by reducing unnecessary checks.

### 19. What is dependency injection in Angular?

Dependency Injection (DI) in Angular is a design pattern and a core feature that allows the injection of dependencies (such as services, objects, or values) into components, services, directives, or other classes instead of creating them manually. This makes the code more modular, reusable, maintainable, and easier to test.

### 20. How Dependency Injection Works in Angular?
1. Define a Service (Dependency):

```
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'  // Service available application-wide (global)
})
export class DataService {
  getData() {
    return 'Some data';
  }
}
```

Inject the Service into a Component:

```
import { Component } from '@angular/core';
import { DataService } from './data.service';

@Component({
  selector: 'app-example',
  templateUrl: './example.component.html'
})
export class ExampleComponent {
  data: string;

  // Dependency injection occurs here: DataService is injected into the component's constructor
  constructor(private dataService: DataService) {
    this.data = this.dataService.getData();
  }
}
```


### 21. How can you pass data between components in Angular?

1. @Input() / @Output(): For passing data between parent and child components.


@Input() (Parent to Child): 
```
// Child Component (child.component.ts)
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-child',
  template: '<p>{{ childMessage }}</p>'
})
export class ChildComponent {
  @Input() childMessage: string;  // Receiving data from parent
}

// Parent Component (parent.component.html)
<app-child [childMessage]="parentMessage"></app-child>
```

@Output() (Child to Parent): 
```
// Child Component (child.component.ts)
import { Component, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-child',
  template: '<button (click)="sendMessage()">Send Message</button>'
})
export class ChildComponent {
  @Output() messageEvent = new EventEmitter<string>();

  sendMessage() {
    this.messageEvent.emit('Hello from Child!');
  }
}

// Parent Component (parent.component.html)
<app-child (messageEvent)="receiveMessage($event)"></app-child>

// Parent Component (parent.component.ts)
export class ParentComponent {
  receiveMessage(message: string) {
    console.log(message);  // 'Hello from Child!'
  }
}
```

2. Shared Service with RxJS: For passing data between sibling or unrelated components.

When you need to pass data between sibling components or unrelated components, a common approach is to use a shared service with RxJS Subjects or BehaviorSubjects to communicate.

Shared Service (message.service.ts):

```
import { Injectable } from '@angular/core';
import { BehaviorSubject } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class MessageService {
  private messageSource = new BehaviorSubject<string>('default message');
  currentMessage = this.messageSource.asObservable();

  changeMessage(message: string) {
    this.messageSource.next(message);
  }
}
```

Component 1 (Sender) (component1.component.ts):


```
import { Component } from '@angular/core';
import { MessageService } from './message.service';

@Component({
  selector: 'app-component1',
  template: '<button (click)="newMessage()">Send Message</button>'
})
export class Component1 {
  constructor(private messageService: MessageService) {}

  newMessage() {
    this.messageService.changeMessage('Hello from Component 1!');
  }
}

```

Component 2 (Receiver) (component2.component.ts):

```
import { Component, OnInit } from '@angular/core';
import { MessageService } from './message.service';

@Component({
  selector: 'app-component2',
  template: '<p>{{ message }}</p>'
})
export class Component2 implements OnInit {
  message: string;

  constructor(private messageService: MessageService) {}

  ngOnInit() {
    this.messageService.currentMessage.subscribe(message => this.message = message);
  }
}
```

3. @ViewChild(): For a parent component to directly access child component properties or methods.

```
// Child Component (child.component.ts)
@Component({
  selector: 'app-child',
  template: '<p>{{ message }}</p>'
})
export class ChildComponent {
  message = 'Hello from Child Component';

  greet() {
    console.log('Hello from Child!');
  }
}

// Parent Component (parent.component.ts)
import { Component, ViewChild } from '@angular/core';
import { ChildComponent } from './child.component';

@Component({
  selector: 'app-parent',
  template: `
    <app-child></app-child>
    <button (click)="callChildMethod()">Call Child Method</button>
  `
})
export class ParentComponent {
  @ViewChild(ChildComponent) childComponent: ChildComponent;

  callChildMethod() {
    this.childComponent.greet();  // Calls child component's method
  }
}
```

4. Router and Route Parameters: For passing data when navigating between different routes.

```
// In routing module
{ path: 'user/:id', component: UserComponent }

// In Parent Component (Link with data)
<a [routerLink]="['/user', userId]">View User</a>

// In User Component (Receiving data)
import { ActivatedRoute } from '@angular/router';

constructor(private route: ActivatedRoute) {
  this.route.params.subscribe(params => {
    const userId = params['id'];  // Access the userId
  });
}
```


### 23. What are Angular lifecycle hooks?
Angular lifecycle hooks are methods that allow you to tap into key moments in a component’s lifecycle. Here are the main lifecycle hooks:

ngOnChanges() - called when input properties change
ngOnInit()  - called after the component is initialized
ngDoCheck() - called on every change detection cycle
ngAfterContentInit() - called after content projection is initialized
ngAfterContentChecked() - called after every change detection cycle for projected content
ngAfterViewInit() - called after the component’s view is initialized
ngAfterViewChecked() - called after every change detection cycle for the component's view
ngOnDestroy() - called just before the component is destroyed


### 24. What are Angular interceptors?
Interceptors are part of the HTTP client module that allow inspection and transformation of HTTP requests and responses.

Uses of Angular Interceptors:
1. Adding Authorization Headers
2. Logging HTTP Requests and Responses
3. Global Error Handling
4. Transforming Requests/Responses


Create the Interceptor:
```
import { Injectable } from '@angular/core';
import { HttpInterceptor, HttpRequest, HttpHandler, HttpEvent } from '@angular/common/http';
import { Observable } from 'rxjs';
import { finalize } from 'rxjs/operators';

@Injectable()
export class MyInterceptor implements HttpInterceptor {
  
  // The intercept method intercepts the outgoing request and allows modification
  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    console.log('Intercepted HTTP call', req);

    // Clone the request and modify it, for example, add an authorization header
    const modifiedReq = req.clone({
      headers: req.headers.set('Authorization', 'Bearer my-token')
    });

    // Pass the modified request to the next handler in the chain
    return next.handle(modifiedReq).pipe(
      finalize(() => {
        console.log('Request completed');
      })
    );
  }
}
```
Provide the Interceptor:
```
import { HTTP_INTERCEPTORS } from '@angular/common/http';
import { MyInterceptor } from './my-interceptor.service';

@NgModule({
  providers: [
    {
      provide: HTTP_INTERCEPTORS,
      useClass: MyInterceptor,
      multi: true  // 'multi: true' allows multiple interceptors to be chained
    }
  ]
})
export class AppModule {}
```


### 25. Explain the purpose of NgZone in Angular.
NgZone in Angular is a service that provides an execution context that allows Angular to detect changes and trigger the change detection cycle efficiently. It helps Angular track asynchronous tasks (like HTTP requests, user interactions, timers, etc.) and update the UI when necessary.

#### Responsibilities of NgZone:
1. Automatic Change Detection: Angular automatically triggers change detection whenever an asynchronous task (such as user input, an HTTP request, or a timer) is completed, keeping the UI in sync with the application state.

2. Optimizing Performance: While NgZone automatically triggers change detection, developers can use NgZone to manually control when Angular runs change detection to improve performance, especially in high-performance or real-time applications.

#### Methods of NgZone:

1. run():
Forces Angular to run change detection when executing a specific piece of code. Useful when performing operations outside Angular’s zone and you need to manually bring it back into the zone to trigger UI updates.
```
this.zone.run(() => {
  // Code that should trigger change detection
});
```

2. runOutsideAngular():

Executes a block of code outside Angular’s zone, meaning change detection is not triggered.
This is useful when you want to perform expensive operations (like setInterval or animations) without Angular continuously running change detection cycles.

```
this.zone.runOutsideAngular(() => {
  setInterval(() => {
    // Code that runs without triggering change detection
  }, 1000);
});
```

3. onStable:

Provides an observable that fires when the application becomes stable (i.e., when all asynchronous tasks have finished).

```
this.zone.onStable.subscribe(() => {
  console.log('App is stable');
});
```


### 26. What is the purpose of NgModule in Angular?

The NgModule in Angular is a decorator that defines a module, which is a container for organizing different parts of an application. It is used to group related components, directives, pipes, and services together, and also helps in managing dependencies by importing and exporting other modules. The NgModule system allows Angular to manage the app's structure and compile it efficiently.

```
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent } from './app.component';
import { FormsModule } from '@angular/forms';

@NgModule({
  declarations: [
    AppComponent  // Declares the component belonging to this module
  ],
  imports: [
    BrowserModule,  // Imports other modules needed by this module
    FormsModule
  ],
  providers: [],   // Services can be provided here
  bootstrap: [AppComponent]  // Root component to bootstrap the application
})
export class AppModule { }
```


* declarations: Declares the components, directives, and pipes that belong to this module.
* imports: Imports other Angular modules whose components, directives, or services are needed.
* exports: Exports components, directives, or pipes to make them available to other modules.
* providers: Registers services that will be available application-wide (or module-wide).
* bootstrap: Specifies the root component that Angular will load and bootstrap at application startup (used only in the root module).


### 27. What is the purpose of Angular animations?
Angular animations provide a way to include animation logic inside Angular applications to enhance user experience.

### 28. Explain dynamic components in Angular.
Dynamic components in Angular are components that are created and inserted into the application at runtime, rather than being statically declared in the template for greater flexibility and responsiveness in applications.

Key Concepts for Dynamic Components in Angular:

1. ComponentFactoryResolver: This service is used to create a component factory for the dynamic component. The factory is then used to instantiate and insert the component into the view.

2. ViewContainerRef: This reference points to a location in the DOM where the dynamic component will be inserted. It is typically injected via @ViewChild to access a container in the component’s template.

3. ComponentRef: Represents the dynamically created component instance. You can use it to interact with the dynamic component, pass data, or destroy it later.


Component to be Loaded Dynamically:
```
import { Component } from '@angular/core';

@Component({
  selector: 'app-dynamic',
  template: `<h3>This is a dynamically loaded component!</h3>`
})
export class DynamicComponent {}
```

Host Component: 
```
import { Component, ViewChild, ViewContainerRef, ComponentFactoryResolver } from '@angular/core';
import { DynamicComponent } from './dynamic.component';

@Component({
  selector: 'app-root',
  template: `
    <div>
      <button (click)="loadComponent()">Load Dynamic Component</button>
      <ng-container #dynamicComponentContainer></ng-container>
    </div>
  `
})
export class AppComponent {
  @ViewChild('dynamicComponentContainer', { read: ViewContainerRef, static: true }) 
  container: ViewContainerRef;

  constructor(private componentFactoryResolver: ComponentFactoryResolver) {}

  loadComponent() {
    const componentFactory = this.componentFactoryResolver.resolveComponentFactory(DynamicComponent);
    this.container.clear();  // Clear any previously rendered components
    this.container.createComponent(componentFactory);  // Create and insert the dynamic component
  }
}
```

Add the Components to the NgModule:
```
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent } from './app.component';
import { DynamicComponent } from './dynamic.component';

@NgModule({
  declarations: [
    AppComponent,
    DynamicComponent  // Declare the dynamic component
  ],
  imports: [BrowserModule],
  entryComponents: [DynamicComponent],  // Important for Angular < 9
  bootstrap: [AppComponent]
})
export class AppModule { }
```

### 28. How can we handle Data from Multiple APIs?

1. Sequential API Calls: One API call depends on the result of another. For example, you first get a list of users, and based on the user ID, you fetch the user's detailed information.

* API 1: Fetch a list of users (/users)
* API 2: Fetch details of a specific user (/users/:id)

switchMap: This operator cancels the previous request when a new one is made and switches to the new observable (API call). It’s useful for dependent API calls where you fetch data from one API and use the response to make another request.

```
import { Component, OnInit } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { switchMap } from 'rxjs/operators';

@Component({
  selector: 'app-root',
  template: `
    <h2>User Details</h2>
    <p *ngIf="userDetails">{{ userDetails | json }}</p>
  `
})
export class AppComponent implements OnInit {
  userDetails: any;

  constructor(private http: HttpClient) {}

  ngOnInit(): void {
    // Fetch a list of users, then fetch details of the first user
    this.http.get('https://jsonplaceholder.typicode.com/users')
      .pipe(
        switchMap((users: any) => {
          const firstUserId = users[0].id;
          return this.http.get(`https://jsonplaceholder.typicode.com/users/${firstUserId}`);
        })
      )
      .subscribe(
        (userDetails) => {
          this.userDetails = userDetails;
        },
        (error) => {
          console.error('Error fetching data:', error);
        }
      );
  }
}
```


2. Parallel API Calls: Multiple API calls are made at the same time, and once all the responses are received, you combine the data or process it.

* API 1: Fetch posts (/posts)
* API 2: Fetch comments (/comments)

forkJoin: This operator is used when you want to wait for all observables to complete and then process the results. It emits a single value that contains the results of all observables once they all complete.

```
import { Component, OnInit } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { forkJoin } from 'rxjs';

@Component({
  selector: 'app-root',
  template: `
    <h2>Posts and Comments</h2>
    <div *ngIf="data">
      <h3>Posts:</h3>
      <p>{{ data.posts | json }}</p>
      <h3>Comments:</h3>
      <p>{{ data.comments | json }}</p>
    </div>
  `
})
export class AppComponent implements OnInit {
  data: any;

  constructor(private http: HttpClient) {}

  ngOnInit(): void {
    // Make two parallel API calls and wait for both responses
    forkJoin({
      posts: this.http.get('https://jsonplaceholder.typicode.com/posts'),
      comments: this.http.get('https://jsonplaceholder.typicode.com/comments')
    })
    .subscribe(
      (result) => {
        this.data = result;
      },
      (error) => {
        console.error('Error fetching data:', error);
      }
    );
  }
}
```

### 29. Debugging Change Detection Issues. If a component is not updating as expected when data changes how can we resolve this issue.

1. Check Angular Zones (NgZone)

Angular uses NgZone to trigger change detection when asynchronous tasks like HTTP requests, user inputs, or timers are completed. Sometimes, third-party libraries or custom asynchronous operations can run outside Angular’s zone, which prevents the view from updating.

```
import { Component, NgZone } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `<p>{{ data }}</p>`
})
export class AppComponent {
  data: string = '';

  constructor(private zone: NgZone) {}

  updateDataOutsideAngular() {
    setTimeout(() => {
      this.data = 'Updated outside Angular zone';
      // The view will not update automatically
    }, 1000);
  }

  updateDataInAngularZone() {
    this.zone.run(() => {
      setTimeout(() => {
        this.data = 'Updated inside Angular zone';
        // The view will update automatically
      }, 1000);
    });
  }
}
```

2. ChangeDetectorRef to Manually Trigger Change Detection


```
import { Component, ChangeDetectorRef } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `<p>{{ data }}</p>`
})
export class AppComponent {
  data: string = 'Initial Data';

  constructor(private cdr: ChangeDetectorRef) {}

  updateData() {
    setTimeout(() => {
      this.data = 'Updated Data';
      this.cdr.detectChanges();  // Manually trigger change detection
    }, 1000);
  }
}
```

* detectChanges(): Forces an immediate check of the component and its children.
* markForCheck(): Marks the component to be checked in the next change detection cycle.

3. Check if the component is using OnPush change detection strategy:

```
@Component({
  selector: 'app-sample',
  templateUrl: './sample.component.html',
  changeDetection: ChangeDetectionStrategy.OnPush
})
```

If the OnPush strategy is being used, Angular only checks for changes when an input reference changes. We are referring to how Angular tracks object references rather than the contents of the object itself. Specifically, Angular uses object identity (the memory reference) to detect changes, rather than deeply inspecting the object's properties. If the data is updated by mutation, Angular will not detect the change. In this case, I would either:

Ensure the object reference is changed, or
Manually trigger change detection using ChangeDetectorRef:

```
constructor(private cd: ChangeDetectorRef) {}

updateData() {
  this.data = { ...this.data, newValue: 'updated' };
  this.cd.markForCheck();  // Manually trigger change detection
}
```

Explanation: If the object is mutated directly, OnPush doesn’t detect the change. Creating a new object or using markForCheck ensures that Angular runs change detection.


### 30. What Does Reference Change Mean?

1. Primitive values (like string, number, boolean):

Angular will detect changes whenever the value itself changes because primitives are compared by value.

`@Input() title: string;`

If title changes from 'Old Title' to 'New Title', Angular will detect this change, as the values are different.

2. Non-primitive values (like object, array):

Angular checks object references to detect changes. If the reference of the input object or array does not change, Angular assumes there’s no change, even if the contents of the object or array are modified.

`@Input() user: { name: string; age: number };`

If user refers to the same object in memory but one of its properties is updated (e.g., user.name = 'New Name'), Angular will not detect this change unless a new object reference is assigned to user.

