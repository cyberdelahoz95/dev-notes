# Basic Notes

Like in most frontend frameworks and libraries, Angular has been designed to offer a tool to create web components that allows us to develop a modularized application.

### Creating Components

To create a component manually, we must first create a folder with the name of the component. Then we create three files, one for template, one for styles and one for typescript. Finally we have to modify the file app.module.ts and import the component and add declarations attribute.

Or we can we the Angular CLI command

```text
ng g c my-awesome-component
```

### Component Lifecycle

* constructor
* ngOnChange
* ngOnInit
  * this method is executed once, it is triggered when the component is rendered, here we can add APIs calls.
* ngOnCheck
* ngOnDestroy
  * This method can be used to kill listeners and avoid memory leaks.

