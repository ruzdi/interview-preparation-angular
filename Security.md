# Angular Route Guards Explained

## 1. What are Angular Guards?

Angular Guards are tools used to control the navigation flow in an Angular application. They allow you to determine whether a user can navigate to or away from certain routes based on specific conditions, adding a layer of control and security.

## 2. What are Route Guards, and What Types are Available in Angular?

Route guards are services that control whether a user can navigate to or away from a route. They implement specific interfaces to control access or functionality. The available types of guards are:

- **CanActivate**: Determines whether a route can be activated.
- **CanActivateChild**: Determines if child routes can be activated.
- **CanDeactivate**: Determines if you can leave the route.
- **Resolve**: Fetches data before the route is activated.
- **CanLoad**: Prevents lazy-loaded modules from being loaded.

### Examples of Route Guards

#### CanActivate

The `CanActivate` guard determines whether a route can be activated or not. This is typically used to restrict access to routes based on certain conditions, such as authentication.

```typescript
@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanActivate {
  constructor(private authService: AuthService) {}

  canActivate(route: ActivatedRouteSnapshot, state: RouterStateSnapshot): boolean {
    return this.authService.isLoggedIn(); // Assume this checks if the user is authenticated
  }
}

const routes: Routes = [
  { path: 'dashboard', component: DashboardComponent, canActivate: [AuthGuard] }
];
```

#### CanActivateChild

The `CanActivateChild` guard is used to determine whether child routes can be activated.

```typescript
@Injectable({
  providedIn: 'root'
})
export class AdminGuard implements CanActivateChild {
  canActivateChild(): boolean {
    return isAdmin(); // Checks if the user has admin privileges
  }
}

const routes: Routes = [
  { path: 'admin', component: AdminComponent, canActivateChild: [AdminGuard], children: [
    { path: 'users', component: UsersComponent },
    { path: 'settings', component: SettingsComponent }
  ]}
];
```

#### CanDeactivate

The `CanDeactivate` guard determines whether the user can leave the current route. This is useful for scenarios like preventing unsaved changes from being lost.

```typescript
@Injectable({
  providedIn: 'root'
})
export class CanDeactivateGuard implements CanDeactivate<UnsavedChangesComponent> {
  canDeactivate(component: UnsavedChangesComponent): boolean {
    return component.hasUnsavedChanges() ? confirm("Do you want to discard your changes?") : true;
  }
}

const routes: Routes = [
  { path: 'form', component: FormComponent, canDeactivate: [CanDeactivateGuard] }
];
```

#### Resolve

The `Resolve` guard is used to pre-fetch data before the route is activated, ensuring the component has the required data when it is rendered.

```typescript
@Injectable({
  providedIn: 'root'
})
export class DataResolver implements Resolve<any> {
  constructor(private dataService: DataService) {}

  resolve(): Observable<any> {
    return this.dataService.getData();
  }
}

const routes: Routes = [
  { path: 'profile', component: ProfileComponent, resolve: { profileData: DataResolver } }
];

constructor(private route: ActivatedRoute) {
  this.route.data.subscribe(data => {
    console.log(data.profileData);
  });
}
```

#### CanLoad

The `CanLoad` guard prevents a module from being loaded lazily until a condition is met, which is useful for large applications where you want to load modules only when the user is authorized.

```typescript
@Injectable({
  providedIn: 'root'
})
export class AuthCanLoadGuard implements CanLoad {
  canLoad(route: Route): boolean {
    return isAuthenticated(); // Loads the module if the user is authenticated
  }
}

const routes: Routes = [
  { path: 'admin', loadChildren: () => import('./admin/admin.module').then(m => m.AdminModule), canLoad: [AuthCanLoadGuard] }
];
```

## 3. Implementing Route Guards for Authentication Using JWT and Adding Tokens to API Calls with Interceptors

### 1. Create the Authentication Service

```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Router } from '@angular/router';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class AuthService {
  private apiUrl = 'https://backend-api.com'; // Replace with your backend API

  constructor(private http: HttpClient, private router: Router) {}

  login(username: string, password: string): Observable<any> {
    return this.http.post(`${this.apiUrl}/auth/login`, { username, password });
  }

  setToken(token: string): void {
    localStorage.setItem('authToken', token);
  }

  getToken(): string | null {
    return localStorage.getItem('authToken');
  }

  isAuthenticated(): boolean {
    const token = this.getToken();
    return !!token;
  }

  logout(): void {
    localStorage.removeItem('authToken');
    this.router.navigate(['/login']);
  }
}
```

### 2. Create the Authentication Guard

```typescript
import { Injectable } from '@angular/core';
import { CanActivate, Router } from '@angular/router';
import { AuthService } from './auth.service';

@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanActivate {
  constructor(private authService: AuthService, private router: Router) {}

  canActivate(): boolean {
    if (this.authService.isAuthenticated()) {
      return true;
    } else {
      this.router.navigate(['/login']);
      return false;
    }
  }
}
```

### 3. Routes with the Guard

```typescript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { AuthGuard } from './auth.guard';
import { DashboardComponent } from './dashboard/dashboard.component';
import { LoginComponent } from './login/login.component';

const routes: Routes = [
  { path: 'login', component: LoginComponent },
  { path: 'dashboard', component: DashboardComponent, canActivate: [AuthGuard] },
  { path: '', redirectTo: '/home', pathMatch: 'full' },
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule {}
```

### 4. Interceptors for API Calls After Login

```typescript
import { Injectable } from '@angular/core';
import { HttpInterceptor, HttpRequest, HttpHandler, HttpEvent } from '@angular/common/http';
import { Observable } from 'rxjs';
import { AuthService } from './auth.service';

@Injectable()
export class TokenInterceptor implements HttpInterceptor {
  constructor(private authService: AuthService) {}

  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    const token = this.authService.getToken();

    if (token) {
      const clonedRequest = req.clone({
        headers: req.headers.set('Authorization', `Bearer ${token}`)
      });
      return next.handle(clonedRequest);
    } else {
      return next.handle(req);
    }
  }
}
```

## Summary

Angular route guards are essential tools for managing navigation, access control, and data preloading in applications. By combining guards such as `CanActivate`, `CanLoad`, and others, you can create a secure and efficient navigation flow that enhances the user experience.