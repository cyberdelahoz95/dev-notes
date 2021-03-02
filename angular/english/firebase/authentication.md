# Authentication

### Adding firebase variables to our app

```typescript
export const environment = {
  production: false,
  url_api: '',
  firebaseConfig: {
    apiKey: '',
    authDomain: '',
    databaseURL: '',
    projectId: '',
    storageBucket: '',
    messagingSenderId: '',
    appId: '',
  },
};
```

### Importing firebase modules

```typescript
import { AngularFireAuthModule } from '@angular/fire/auth';
import { BrowserAnimationsModule } from '@angular/platform-browser/animations';
import { environment } from './../environments/environment';

@NgModule({
//declarations
  imports: [
    //other modules
    AngularFireModule.initializeApp(environment.firebaseConfig),
    AngularFireAuthModule
  ],
  // providers: [],
  // bootstrap: [AppComponent],
})
export class AppModule {}
```

### Adding our fire module to an auth service

```typescript
import { Injectable } from '@angular/core';
import { AngularFireAuth } from '@angular/fire/auth';

@Injectable({
  providedIn: 'root',
})
export class AuthService {
  constructor(private fireAuth: AngularFireAuth) {}

  createUser(email: string, password: string) {
    return this.fireAuth.createUserWithEmailAndPassword(email, password);
  }
  login(email: string, password: string) {
    return this.fireAuth.signInWithEmailAndPassword(email, password);
  }
  logout() {
    return this.fireAuth.signOut();
  }
  hasUser() {
    return this.fireAuth.authState;
  }
}

```

### Consuming our service in a login component

```typescript
  login(event: Event) {
    event.preventDefault();
    if (this.form.valid) {
      const value = this.form.value;
      this.authService
        .login(value.email, value.password)
        .then(() => {
          this.router.navigate(['/admin']);
        })
        .catch(() => {
          alert('Invalid credentials');
        });
    }
  }
```

### Bonus

The following commands are useful to deploy our to firebase

```bash
firebase init
ng build --prod
firebase deploy
```

The following lines can be added to firebase.json in order to avoid not found exception in case we want to go to an specific path directly in the browser.

```bash
"hosting": {
  // ...

  // Serves index.html for requests to files or directories that do not exist
  "rewrites": [ {
    "source": "**",
    "destination": "/index.html"
  } ]
}
```

