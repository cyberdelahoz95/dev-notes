# Environment

Angular has 2 built-in environments, but we can create our own environments.

### Steps to create new environment

Before starting it is recommended to run 

_**ng build  --prod**_

This step is very useful to identify any error in our code in terms of imports of modules or dependency injections. This process uses whatevery we have configured in our angular.json file. it is not recommended to modify configurations related with the built-in environments. If we detect some problem when running the build, we want to fix it before creating any nre environment.

1. Create a new ts file in our environment folder.
2. Open angular.json file and create a new key value pair value in the configurations object. This key will be the name of our new environment.The value of this pair is going to be all our configurations for our new environment.
3. Modify the key _**serve**_ from angular.json we add a new entry with the new of our new environment.
4. we can run the command _**ng serve -c=newEnv**_ or _**ng build -c=newEnv**_ to use our newly created environment file.

