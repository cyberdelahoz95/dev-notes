# Intro

### FormModule

One of the tools we have available to build forms in Angular is using FormModule, this module provides directive that can be used in the view in order to connect the data with the controller. The most common directive to achieve the previous description is ngModel.

### ReactiveFormModule

It is a more powerful module to build forms in Angular. 

In order to connect data from views to controller and vice versa, we can use FormControl class.

Using a FormControl class allows us to subscribe to changes in our form, for example when an input field is populated. Being subscribe to such events enables us to react in different ways in order to process our data.

Additionally, being subscripted to changes in the form data enables us to perform validations and implement observables to extend the behavior of the data flow, for example we can modify the data before is received by the controller or before sending the data to our backend.

