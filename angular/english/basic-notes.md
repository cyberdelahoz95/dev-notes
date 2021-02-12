# Basic Notes

Like in most frontend frameworks and libraries, Angular has been designed to offer a tool to create web components that allows us to develop a modularized application.

## Creating Components

To create a component manually, we must first create a folder with the name of the component. Then we create three files, one for template, one for styles and one for typescript. Finally we have to modify the file app.module.ts and import the component and add declarations attribute.

Or we can we the Angular CLI command

```bash
ng g c my-awesome-component
```

## Component Lifecycle

* constructor
* ngOnChange
* ngOnInit
  * this method is executed once, it is triggered when the component is rendered, here we can add APIs calls.
* ngOnCheck
* ngOnDestroy
  * This method can be used to kill listeners and avoid memory leaks.

## How data flows from one component to another

```javascript
@Input() item: string;
```

@Input is the way Angular makes data flows from parents components to its children components.

```javascript
@Output() newItemEvent = new EventEmitter<string>();
```

@Output is the way Angular makes data flows from children components to their parents. Also Output fires events from the children components that can be handle by the parent.

## Linting

We can use the CLI command to fire linting

```bash
ng lint
```

## Modules

Modules can be created by the Angular CLI command

```bash
ng g m my-awesome-module
```

a module can contain its own set of routes, this can be done by adding the following flag when the module is created

```bash
ng g m my-awesome-module --routing
```

## Directives

Directives are use to manipulate an element from the DOM, it can be use to change the native behavior of an element.

It can be created using CLI

```bash
ng g d my-awesome-directive
```

similarly, custom pipes can be created using the CLI

```bash
ng g p my-awesome-pipe
```

## Services

Angular services are used as data providers to modules and components. Services can be created using the CLI command

```bash
ng g s my-awesome-service
```

## Dependency Injection

Angular also provides an easy way to inject dependencies and reduce the complexity of the components and modules. One way DI can be implemented is by adding the dependency as a class attribute via the constructor.

```javascript
constructor(private fb: FormBuilder) {}
```

## Lazy Loading

To implement LL in Angular the app needs to be modularized. One way this can be done is by implementing main pages \(functionalities\) as a module, when this is done, all the required functions, libraries, components, directives, pipes etc. are encapsulated.

Once the app is modularized then the file app-routing.module.ts will look like this sample

```javascript
{
    path: 'home',
    loadChildren: () =>
        import('./home/home.module')
            .then((m) => m.HomeModule),
}
```

## Guards

Guards can be used as middleware, one of the benefits of guards is to protect certain routes. So user can only have access to certain routes if guard allows the request to proceed.

Guards can be created using the command

```bash
ng g g my-awesome-guard
```

```javascript
import { Injectable } from '@angular/core';
import {
  CanActivate,
  ActivatedRouteSnapshot,
  RouterStateSnapshot,
  UrlTree,
} from '@angular/router';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root',
})
export class AdminGuard implements CanActivate {
  canActivate(
    next: ActivatedRouteSnapshot,
    state: RouterStateSnapshot
  ):
    | Observable<boolean | UrlTree>
    | Promise<boolean | UrlTree>
    | boolean
    | UrlTree {
    return true;
  }
}
```

## HTTP Requests

Angular provides it´s own module to process http request. It is important to remember that the http module of angular returns Observable objects, so in order to consume the result of the request we will need to use the method subscribe in order to have access to the data.

```javascript
  fetchProduct() {
    this.productsService.getAllProducts().subscribe((products) => {
      this.products = products;
    });
  }
```

We can easily map the response from a http request by declaring the desired type next to the method name.

```javascript
  getAllProducts() {
    return this.http.get<Product[]>(environment.url_api);
  }
```

the previous approach can be applied in many scenarios.

## Partial

Partial can be used as a wrapper of any type. Partial set all the attributes from a type as optional. This is very useful when we want to perform patch request in order to update a record in the database.

```javascript
  updateProduct(id: string, set: Partial<Product>) {
    return this.http.put(`${environment.url_api}/${id}`, set);
  }
```

## Ivy

This is the main renderer built by Angular. This technology helps to build small size apps, faster compilation, easier and faster debugging.

Ivy provides rules to automatically delete portions of Angular that are not used by the app. We do not have to transport the whole size framework for all apps, we ship the code we need.

## Differential Loading

This feature of Angular is a mechanism to reduce the number of files downloaded by the client. 

When Angular detects the kind of browser the client is using \(version, features of the browser, etc.\) select which polyfill should be downloaded. 

Remember a polyfill is like a patch used to enhance the browser and allow modern web apps to run in old browsers.

Modern browsers require less polyfills and viceversa.

