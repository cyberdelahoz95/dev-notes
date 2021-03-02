# Typing HTTP responses

We can use an interface to assign a valid data type to the response comming from an API. This is a good practice to avoid certain type problems.

As an example to show how to type http response, let's give our attention to the following interface.

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

