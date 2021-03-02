# Reactivity with RXjs

RxJS is a library that allows us to handle reactivity all over the app via subscription publisher pattern.

One common class that we can use to implement this funcionality is _**BehaviorSubject**_ class

```typescript
import { Injectable } from '@angular/core';
import { BehaviorSubject } from 'rxjs'; 
import { Product } from './../../core/models/product.model';

@Injectable({
  providedIn: 'root',
})
export class CartService {
  // value to be observed
  private products: Product[] = [];
  //Observable objeto that sets a channel to convey data to observers
  private cart = new BehaviorSubject<Product[]>([]);

  constructor() {}

  addToCart(product: Product) {
    // setting new value
    this.products = [...this.products, product];
    // sending new value to suscriptors
    this.cart.next(this.products);
  }
}
```



