# Angular Router Interview Questions

## 1. What is Angular Router?

Angular Router is a core module that helps manage navigation between different views or components in an Angular application. It allows for client-side navigation by mapping routes (URLs) to specific components.

```typescript
const routes: Routes = [
  { path: 'home', component: HomeComponent },
  { path: 'about', component: AboutComponent },
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule {}
```

## 2. What is `RouterModule.forRoot()` and `RouterModule.forChild()`?

- `RouterModule.forRoot()` is used to register the routes at the application level (i.e., root-level routes). It is typically called in the root module (`AppModule`).
- `RouterModule.forChild()` is used for feature modules to define their own child routes.

Example:

```typescript
// App routing (AppModule)
RouterModule.forRoot(appRoutes);

// Feature module routing (FeatureModule)
RouterModule.forChild(featureRoutes);
```

## 3. How can you pass data to a route using Angular Router?

- **Route Parameters**: Pass dynamic data via the URL (e.g., `/user/:id`).
- **Route Data**: Static data can be passed using the `data` property in the route configuration.

```typescript
const routes: Routes = [
  { path: 'user/:id', component: UserComponent },
  { path: 'dashboard', component: DashboardComponent, data: { title: 'Dashboard' } }
];
```

**UserComponent**:

```typescript
constructor(private route: ActivatedRoute) {
  this.route.params.subscribe(params => {
    console.log(params['id']);  // Access the ID
  });
}
```

**DashboardComponent**:

```typescript
constructor(private route: ActivatedRoute) {
  console.log(this.route.snapshot.data['title']);  // Access the data
}
```

## 4. How can you navigate programmatically in Angular?

Use the `Router` service's `navigate()` or `navigateByUrl()` methods to programmatically navigate between routes.

- **navigate()**: Takes an array of URL segments and works with route configurations, allowing relative navigation.
- **navigateByUrl()**: Takes a full URL string and is typically used for absolute navigation.

```typescript
constructor(private router: Router) {}

navigateToHome() {
  this.router.navigate(['/home']); // Relative navigation

}

navigateToUser() {
  this.router.navigateByUrl('/user/123'); // Absolute navigation
}
```

## 5. What is lazy loading in Angular, and how do you implement it?

Lazy loading is a technique where a module or component is loaded only when it's needed, reducing the initial load time of the application.

Example: To implement lazy loading in Angular, define routes using `loadChildren`:

```typescript
const routes: Routes = [
  { path: 'admin', loadChildren: () => import('./admin/admin.module').then(m => m.AdminModule) }
];
```

## 6. What is `ActivatedRoute` and how is it used?

`ActivatedRoute` is a service that provides access to information about the current route, including parameters, static data, query parameters, and more.

```typescript
constructor(private route: ActivatedRoute) {
  // Access route parameters
  this.route.params.subscribe(params => {
    console.log(params['id']);
  });

  // Access query parameters
  this.route.queryParams.subscribe(queryParams => {
    console.log(queryParams['page']);
  });
}
```

## 7. How can you define a default route or a wildcard route in Angular?

```typescript
const routes: Routes = [
  { path: '', redirectTo: '/home', pathMatch: 'full' },  // Default route
  { path: '**', component: PageNotFoundComponent }       // Wildcard route
];
```

## 8. How do you configure nested routes (child routes) in Angular?

Angular allows you to define child routes within a parent route using the `children` property in the route configuration.

```typescript
const routes: Routes = [
  {
    path: 'parent',
    component: ParentComponent,
    children: [
      { path: 'child1', component: Child1Component },
      { path: 'child2', component: Child2Component }
    ]
  }
];
```

## 9. What is `RouterLink` and `RouterLinkActive` in Angular?

- **RouterLink**: Used for navigation via templates, similar to the `href` attribute in HTML but with Angular's router.
- **RouterLinkActive**: Adds a class to the link's element when its route is active, useful for highlighting the current navigation.

```html
<a [routerLink]="['/home']" routerLinkActive="active">Home</a>
```

## 10. What is a Resolver in Angular?

A Resolver in Angular is a service that pre-fetches data before a route is activated. It ensures that necessary data is loaded and available for the component associated with the route before the component is instantiated.

- Pre-fetches data for a route before the component is activated.
- Pauses route activation until the data is available.
- Integrated with Angular's router, making it seamless to use in routing configurations.

Example:

```typescript
import { Injectable } from '@angular/core';
import { Resolve, ActivatedRouteSnapshot, RouterStateSnapshot } from '@angular/router';
import { Observable } from 'rxjs';
import { DataService } from './data.service';

@Injectable({
  providedIn: 'root'
})
export class MyResolver implements Resolve<any> {
  constructor(private dataService: DataService) {}

  resolve(route: ActivatedRouteSnapshot, state: RouterStateSnapshot): Observable<any> {
    return this.dataService.getData();  // Fetching data before route activation
  }
}
```

**Configure the Resolver in Routing**:

```typescript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { MyComponent } from './my.component';
import { MyResolver } from './my-resolver.service';

const routes: Routes = [
  {
    path: 'my-route',
    component: MyComponent,
    resolve: {
      data: MyResolver  // The resolver is specified here
    }
  }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule {}
```

**Access the Resolved Data in the Component**:

```typescript
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute } from '@angular/router';

@Component({
  selector: 'app-my-component',
  template: `<p>{{ data | json }}</p>`
})
export class MyComponent implements OnInit {
  data: any;

  constructor(private route: ActivatedRoute) {}

  ngOnInit() {
    this.data = this.route.snapshot.data['data'];  // Accessing the resolved data
  }
}
```

## 11. How do you handle route animations in Angular?

You can handle route animations using Angular's `@angular/animations` library by defining route-specific animations in the component.

Example:

```typescript
const routes: Routes = [
  { path: 'home', component: HomeComponent, data: { animation: 'HomePage' } }
];
```

In the component:

```typescript
animations: [
  trigger('routeAnimation', [
    transition('HomePage <=> AboutPage', [
      style({ opacity: 0 }),
      animate('500ms ease-in', style({ opacity: 1 }))
    ])
  ])
]