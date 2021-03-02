# Reactivity with RXjs

RxJS is a library that allows us to handle reactivity all over the app via subscription publisher pattern.

One common class that we can use to implement this functionality is _**BehaviorSubject**_ class

```typescript
import { Injectable } from '@angular/core';
import { BehaviorSubject } from 'rxjs'; 
import { Product } from './../../core/models/product.model';

@Injectable({
  providedIn: 'root',
})
export class CartService {
  // This value is the dynamic value, we want to react to its changes
  private products: Product[] = [];
  // This BehaviorSubject instance defines the way reactivity is going to be hanle
  private cart = new BehaviorSubject<Product[]>([]);
  // Observable object consume by subscriptors
  cart$ = this.cart.asObservable();

  constructor() {}

  addToCart(product: Product) {
    // setting new value
    this.products = [...this.products, product];
    // sending new value to suscriptors
    this.cart.next(this.products);
  }
}
```

addToCart method can be used from any component injecting the service.

Now how can we subscribe to this reactive class?

```javascript
import { Component, OnInit } from '@angular/core';
import { map } from 'rxjs/operators';
import { CartService } from '@core/services/cart.service';
import { Observable } from 'rxjs';

@Component({
  selector: 'app-header',
  templateUrl: './header.component.html',
  styleUrls: ['./header.component.scss'],
})
export class HeaderComponent implements OnInit {
  total$: Observable<number>;

  constructor(private cartService: CartService) {
    this.total$ = this.cartService.cart$.pipe(
      map((products) => {
        return products.length;
      })
    );
  }

  ngOnInit(): void {}
}
```

### Using switchMap to reduce the number of subscriptions

Code we want to optimize

```typescript
product: Product;

ngOnInit(): void {
    this.route.params.subscribe((params: Params) => {
        const id = params.id;
        this.fetchProduct(id);
    });
}

fetchProduct(id: string) {
    this.productsService.getProduct(id).subscribe((product) => {
        this.product = product;
    });
}
```

Optimized code

```typescript
product$: Observable<Product>;

constructor(
    private route: ActivatedRoute,
    private productsService: ProductsService
) {}

ngOnInit(): void {
    this.product$ = this.route.params.pipe(
        switchMap(
            (params: Params) => this.productsService.getProduct(params.id)
            )
        );
}
```

Consuming the observable in a component

```markup
<div *ngIf="(product$ | async) as product">
```

### Handling errors in subscriptions

```typescript
subscribe(successFunction,
    error=>{
    handlingError();
})
```

We can use the function handlingError to show logs or event store the event or send it to sentry, etc.

