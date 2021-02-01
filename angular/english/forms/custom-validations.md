# Custom Validations

Custom validations can be achieved by creating a class that expose methods that perform the revision of a control value.

It is worth noticing that if we are working with typescript, the validation method is a static one, in other words, the method can be called without instantiating the class.

```typescript
import { AbstractControl } from '@angular/forms';

export class CustomValidators {
  static isValidPrice(control: AbstractControl) {
    const value = control.value;
    if (value > 1000) {
      return { invalid_price: true };
    }
    return null;
  }
}
```

AbstractControl is use to give us access to our control \(what we want to validate\).

One way we can use our custom validation is by adding them as elements of our validators during form groups creation.

```typescript
  private buildForm() {
    this.form = this.fb.group({
      id: ['', [Validators.required]],
      title: ['', [Validators.required]],
      price: ['', [Validators.required, 
        CustomValidators.isValidPrice]],
      image: '',
      description: ['', [Validators.required]],
    });
  }
```

Then we can add error messages in the view

```markup
<p *ngIf="form.get('price').hasError('invalid_price')">
    Required Price
</p>
<p *ngIf="form.get('price').hasError('required')">
    Invalid Price
</p>
```

