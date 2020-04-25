[Home](README.md)

# Add Common Site Views (Pages)

## Add a home view

From the terminal window, run the following command line statement:

```
ng g c home
```

You should see output similar to the following:

![image.png](/.attachments/image-5283ff4d-c597-400d-8376-e1498224b34a.png)

In VS Code, replace the contents of the home.compont.html file with the following:

``` html
<h1>Welcome to my app!</h1> 
<p> 
  We have a lot of fun and educational activities planned for you. 
</p> 
<p> 
  We hope you enjoy your time here at my app. 
</p> 
<p> 
    <button type="button" class="btn btn-primary" [routerLink]="['/error']">Error</button> 
</p> 
<p> 
    <button type="button" class="btn btn-primary" [routerLink]="['/not-found']">Not Found</button> 
</p> 
```

## Add an error view

From the terminal window, run the following command line statement:

```
ng g c error
```

You should see output similar to the following:

![image.png](/.attachments/image-bdb65d22-a77a-4703-b147-2b5a20ff5565.png)

In VS Code, replace the contents of the home.compont.html file with the following:

``` html
<h1>Error</h1>
<p>
    We're sorrry, but something went wrong.
</p>
<p>
    Please try your last action again.
</p>
<p>
    If the problem persists, please contact support for assistance.
</p>
<p>
    <button type="button" class="btn btn-primary" [routerLink]="['/home']">Home</button>
</p>
```

## Add a not-found view

From the terminal window, run the following command line statement:

```
ng g c not-found
```

You should see output similar to the following:

![image.png](/.attachments/image-bdb65d22-a77a-4703-b147-2b5a20ff5565.png)

In VS Code, replace the contents of the home.compont.html file with the following:

``` html
<h1>Not Found</h1>
<p>
    We're sorry, but we are unable to provide the location you requested.
</p>
<p>
    Please submit a request for a different location.
</p>
<p>
    <button type="button" class="btn btn-primary" [routerLink]="['/home']">Home</button>
</p>
```

## Add routes for the new views

From VS Code, edit the app-routing.module.ts file. 

Replace the code with the following:

``` typescript
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';

import { HomeComponent } from './home/home.component';
import { ErrorComponent } from './error/error.component';
import { NotFoundComponent } from './not-found/not-found.component';

const routes: Routes = [
  {
    path: 'home',
    component: HomeComponent,
    data: {
      title: 'Welcome!'
    }
  },
  {
    path: 'error',
    component: ErrorComponent,
    data: {
      title: 'Error'
    }
  },
  {
    path: '',
    redirectTo: '/home',
    pathMatch: 'full'
  },
  {
    path: '**',
    component: NotFoundComponent,
    data: {
      title: 'Not Found'
    }
  }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

## Configure the app.component.html for routing

In VS Code, replace the contents of the app.compont.html file with the following:

``` html
<div>
    <router-outlet></router-outlet>
</div>
```

In VS Code, add the following styles to the **styles.scss** file:

``` scss
/* You can add global styles to this file, and also import other style files */

@import 'variables'; 
@import 'bootstrap';

.spacer-5 {
    height: 5px;
}

.spacer-10 {
    height: 10px;
}

.spacer-15 {
    height: 15px;
}

.spacer-20 {
    height: 20px;
}

.spacer-25 {
    height: 25px;
}

.spacer-30 {
    height: 30px;
}
```

Run the app again.

```
npm run start
```

You should see something like the following:

![image.png](/.attachments/image-343f017a-6af3-44ea-9a15-9a4e2589a16c.png)

You should now be able to click on the buttons to navigate among the newly added views.

[Home](README.md)

[Create The Site Layout](Create-The-Site-Layout.md)