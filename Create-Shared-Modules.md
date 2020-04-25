[Home](README.md)

# Create Shared Modules

Services, components, directives, pipes, etc., can also be modularized and shared among your website pages and components. 

For more information on Angular modules, see [NgModules](https://angular.io/guide/ngmodule#shared-modules). 

## Create A Services Module 

Most of your services should be global singletons. 

From the terminal window, create a module for globally available services: 

```
ng g m services
```

In VS Code, navigate to the new **services.module.ts file** in the **src/app/services** folder.

![image.png](/.attachments/image-5e943e9f-b498-4cfd-808e-73dcd18865fd.png)

The following code was generated:

![image.png](/.attachments/image-04e1fbec-0584-445f-befe-c8fea9c817c3.png)

Edit the services.module.ts code as follows:

``` typescript
import { NgModule, ModuleWithProviders, Optional, SkipSelf } from '@angular/core';
import { CommonModule } from '@angular/common';

@NgModule({
  declarations: [],
  imports: [
    CommonModule
  ]
})
export class ServicesModule {
  // Guard against a lazy loaded module re-importing this module.
  constructor(@Optional() @SkipSelf() parentModule: ServicesModule) {
    if (parentModule) {
      throw new Error(
        'ServicesModule is already loaded. Import it in the AppModule only');
    }
  }

  // The forRoot() pattern ensures services are singletons
  static forRoot(): ModuleWithProviders<ServicesModule> {
    return {
      ngModule: ServicesModule,
      providers: [
      ]
    };
  }
}
```

For more information, see [Prevent reimport of the GreetingModule](https://angular.io/guide/singleton-services#prevent-reimport-of-the-greetingmodule) and [The forRoot() pattern](https://angular.io/guide/singleton-services#the-forroot-pattern).

## Create A Components Module 

From the terminal window, create a module for shared components: 

```
ng g m components
```

In VS Code, navigate to the new **components.module.ts** file in the **src/app/components** folder.

![image.png](/.attachments/image-8e73b190-8c1e-4a9b-809a-fe7457f0d439.png)

The following code was generated:

![image.png](/.attachments/image-53790796-d6e0-4fa8-a42b-bc7eacbf1972.png)

Edit the services.module.ts code as follows:

``` typescript
import { NgModule, ModuleWithProviders, Optional, SkipSelf } from '@angular/core';
import { CommonModule } from '@angular/common';
import { RouterModule } from '@angular/router';
import { FormsModule, ReactiveFormsModule } from '@angular/forms';

@NgModule({
  declarations: [
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
    ReactiveFormsModule
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

Note the inclusion of the RouterModule, FormsModule, and ReactiveFormsModule needed to support common page and component configuration needs.

## Added Shared Modules to the main app.module.ts file

In VS Code, navigate to the new **app.module.ts** file in the **src/app** folder.

Edit the module as shown in the following:

![image.png](/.attachments/image-a61b37cd-70e4-401f-97b1-ba2e643957ad.png)

``` typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { ServicesModule } from './services/services.module';
import { ComponentsModule } from './components/components.module';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    ServicesModule.forRoot(),
    ComponentsModule.forRoot(),
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

You have now created shared modules from which to share services and components. 




