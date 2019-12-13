# zidal-angular4 
A derivative work of adal-angular4, adapted to the [ZData IDentity Server Authentication Library](https://github.com/herkit/zdata-identity-server-authentication-library)

___

Angular 4+ Zidal wrapper package. Can be used to authenticate Angular applications against ZData Identity Server endpoint.
___

## Change Log

### 4.0.9

- Upgraded to Gulp version 4. Build options are now:

```
gulp watch    - watch for file changes do the build task
gulp build    - clean the dist directory and build the project
gulp commit   - bump version and add and commit files to git (for maintainers only)
gulp publish  - publish new npm version (for maintainers only)
```
### 4.0.1

- Updated to support latest version of adal-angular. Major version updated because of potentially breaking changes.

### 3.0.7

- Added an automatic login token refresh feature. It will refresh tokens at application load if there is a valid sign-in token. It will also refresh the login token 5 minutes before it expires.

### 3.0.1

- Updated to Angular 6, cleaned up files. THIS IS A BREAKING VERSION!

### 2.0.0

- Updated to Angular 5, cleaned up files. THIS IS A BREAKING VERSION!

### 1.1.11

- Fixed a bug where the valid scenario of refreshing an id_token is not handled - thanks to @alan-g-chen.

### 1.1.4

- Hash is now removed from url after login

### 1.0.1

- Added HTTP Interceptor for Angular 4.3.0+
- Updated all packages to newest versions

### Update and Build Instructions

```
git clone https://github.com/benbaran/adal-angular4.git

npm install -g @angular/cli@latest gulp@latest

del .\package-lock.json

ng update --all --force

npm install typescript@3.1.1

gulp build
```

### NPM Publish Instructions (For Maintainers Only)
```
git clone https://github.com/benbaran/adal-angular4.git

npm install -g @angular/cli@latest gulp@latest

del .\package-lock.json

ng update --all --force

npm install typescript@3.1.1

gulp publish
```

### Usage ( tested with Angular 8)

First install the package ( ex using npm )
```
npm i adal-angular4
```

Implement ADAL authentication:

## app.module.ts
Open your Angular root module, usually app.module.ts
Add the following import

```Javascript
/*Authentication*/
import { AdalService, AdalGuard, AdalInterceptor } from 'adal-angular4';
import { HTTP_INTERCEPTORS } from '@angular/common/http';
```

Update your @NgModule providers section with the following line:
```JSON
providers: [AdalService, AdalGuard, {provide: HTTP_INTERCEPTORS, useClass: AdalInterceptor, multi: true }
```

It should look something like this:
```Javascript
@NgModule({
  declarations: [
    AppComponent,
    ...
  ],
  imports: [
    ...
  ],
  providers: [AdalService, AdalGuard, {provide: HTTP_INTERCEPTORS, useClass: AdalInterceptor, multi: true }, ... ],
  bootstrap: [AppComponent]
})
```
## environment.ts
inside your environment.ts file add your config, this file is created for you when u use the Angular CLI

Usually can be found here:
src
    ├── environements
        └── environment.ts
        └── environment.prod.ts


```Javascript
export const environment = {
  production: false,
  config: {
    tenant: 'tenant.onmicrosoft.com',
    clientId: 'app registration id',
    endpoints: {
      'http://localhost:4200/': 'the id'
    }
  }
};
```
## app.component.ts
Then import the following into your root component usually app.component.ts

```Javascript
import { environment } from '../environments/environment';
import { AdalService } from 'adal-angular4';
```

Init the adal lib by adding the following lines to your constructor or OnInit

```Javascript
export class AppComponent implements {
    constructor(private adalService: AdalService) {
        this.adalService.init(environment.config);
        this.adalService.handleWindowCallback();
    }
}
```

## app-routing.module.ts
You can protect routes by adding the authguard to you route

import the following inside your routing module file usually app-routing.module.ts
```Javascript
import { AdalGuard } from 'adal-angular4';
```


ex: 
```Javascript
const routes: Routes = [
  {
    path: '',
    component: YourComponent,
    canActivate: [AdalGuard]
  }
];
```

## Login / Logout
You can call the login and logout function with the following code.

AdalService needs to be imported in your file
```Javascript
import { AdalService } from 'adal-angular4';
/*Dont forget to initialize*/
constructor(private adalService: AdalService)
```

**Login:**
```Javascript
this.adalService.login();
```

**Logout:**
```Javascript
this.adalService.logOut();
```

### Check if user is allready authenticated, if not login
Checking if user has allready logged in if not do something

AdalService needs to be imported in your file
```Javascript
import { AdalService } from 'adal-angular4';
/*Dont forget to initialize*/
constructor(private adalService: AdalService)
```

```Javascript
if (this.adalService.userInfo.authenticated) {
    /*All good*/
} else {
    /*No good*/
}
```
