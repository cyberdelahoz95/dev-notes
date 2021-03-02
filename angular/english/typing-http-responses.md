# HTTP responses

### Typing HTTP Responses

We can use an interface to assign a valid data type to the response coming from an API. This is a good practice to avoid certain type problems.

As an example to show how to type HTTP response, let's give our attention to the following interface.

```typescript
interface User {
    gender: string,
    email : string,
    phone : string,
}
```

User interface can be use as follows

```typescript
myFunction(): Observable<User[]>{
    return this.http.get("URL").pipe(
        map(
            (response:any) => response.results as User[]
            ))
}
```

### Handling errors

```typescript
this.http.get()
    .pipe(
        retry(3), // try again 3 times
        catchError(error => {
            return throwError("mensaje de error")
        }),
        //normal flow
    )
```

We use retry operator to avoid firing error immediately, instead we try 3 times and in case no success we process to call catchError operator to handle the exception.

### Downloading files

```typescript
getFile(){
    return this.http.get('URL.txt', {responseType:'text'})
        .subscribe(content => {})
}
```

Inside subscribe method we can handle a way of open a window or any other functionality to give the use the functionality to save the file

