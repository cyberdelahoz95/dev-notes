# JIT vs AOT

Just in time is a compilation technique used by angular in development time, It provides the browser with an Angular compiler so it can understands the code and executes it. This enables the ability of hot reloading, and speed coding up.

Ahead of time is used in production time, it does not provides a compiler for the code because in the building process everything is transformed to js css and html.

AOT is focused on performance, so it gives a very optimized code so that the browser does not spend to much time in parsing and compiles it.

AOT also provides an small size bundle, this is because the compiler is not included in the bundle.

AOT also reduced the requests for static files. For example, notice the following code.

```typescript
@Component({
  selector: 'app-products',
  templateUrl: './products.container.html',
  styleUrls: ['./products.container.scss'],
})
```

when we use JIT compilation, the html and css files are requested via HTTP so that app loading time is faster, this is done like that because basically client and static files server are in the same local machine. On the other hand when we use AOT this files are not included using http request but basically the compiler creates a string version of the files and it includes them in the bundle.

When executing AOT compilation, we have the benefit of getting errors report in case we are not implementing a method in the component definition. This reduces the risk of sending bugs to production.

And it is very nice that AOT also improves the security of our code by applying code obfuscation.

How do we make sure that our code is compiled using AOT ?? By making sure that in our angular.json file we have set the following properties

```typescript
"configurations": {
"production": {
  ...
  "aot": true,
  "buildOptimizer": true,
  ...
  }
}
```

