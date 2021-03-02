# Environment

Angular has 2 built-in environments, but we can create our own environments.

### Steps to create new environment

Before starting it is recommended to run 

_**ng build  --prod**_

This step is very useful to identify any error in our code in terms of imports of modules or dependency injections. This process uses whatevery we have configured in our angular.json file. it is not recommended to modify configurations related with the built-in environments. If we detect some problem when running the build, we want to fix it before creating any nre environment.



