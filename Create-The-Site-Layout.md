# Add A Page Header Component

From the terminal window, run the following command:

```
ng g c components\page-header
```

You should see something like the following in the output:

![image.png](/.attachments/image-e1c97dcd-e5d3-4784-91da-477968cab2ad.png)

Edit the **page-header.component.html** file. 

Replace the content with the following: 

``` html
<nav class="navbar navbar-expand-md navbar-dark bg-dark fixed-top">
    <a class="navbar-brand" href="#">My App</a>
    <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarsExampleDefault"
        aria-controls="navbarsExampleDefault" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarsExampleDefault">
        <ul class="navbar-nav mr-auto">
            <li class="nav-item">
                <a class="nav-link" href="#" routerLink="/home" routerLinkActive="active">Home <span class="sr-only">(current)</span></a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="#" routerLink="/dashboard" routerLinkActive="active">Dashboard</a>
            </li>
        </ul>
    </div>
</nav>
```

# Add A Page Footer Component 

From the terminal window, run the following command:

```
ng g c components\page-footer
```

You should see something like the following in the output:

![image.png](/.attachments/image-51b0cebf-453e-433d-9fb0-7972597ce0ab.png)

Edit the **page-footer.component.html** file. 

Replace the content with the following:

``` html
<footer>
    <div class="m-3">
        My App &copy; 2020
    </div>
</footer>
```

Edit the **components.module.ts** file. 

Modify the code as shown in the following:

![image.png](/.attachments/image-69131688-adc1-485d-bb77-2df91ef62ec8.png)

``` typescript
import { NgModule, ModuleWithProviders } from '@angular/core';
import { CommonModule } from '@angular/common';
import { RouterModule } from '@angular/router';
import { FormsModule, ReactiveFormsModule } from '@angular/forms';

import { PageHeaderComponent } from './page-header/page-header.component';
import { PageFooterComponent } from './page-footer/page-footer.component';

@NgModule({
  declarations: [
    PageHeaderComponent,
    PageFooterComponent
  ],
  imports: [
    CommonModule,
    RouterModule,
    FormsModule,
    ReactiveFormsModule
  ],
  exports: [
    CommonModule,
    RouterModule,
    FormsModule,
    ReactiveFormsModule,
    PageHeaderComponent,
    PageFooterComponent
  ]
})
export class ComponentsModule {
  // The forRoot() pattern ensures services are singletons
  static forRoot(): ModuleWithProviders<ComponentsModule> {
    return {
      ngModule: ComponentsModule,
      providers: [

      ]
    };
  }
}
```

Edit the **app.component.html** file. 

Replace its contents with the following:

``` html
<app-page-header></app-page-header>
<div class="spacer-30"></div>
<div class="container-fluid">  
  <div class="spacer-30"></div>
  <main>
    <router-outlet></router-outlet>
  </main>
</div>
<app-page-footer></app-page-footer>
<div class="spacer-15"></div>
```

Edit the **_variables.scss** file. 

Add the following style override:

``` scss
$primary: #6E0D11; 
```

Run the app again.

```
npm run start
```

You should see something like the following:

![image.png](/.attachments/image-e3ceedd2-9388-4198-9da0-da7234b0f0cb.png)

