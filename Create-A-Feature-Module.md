[Home](README.md)

# Create A Feature Module

Feature modules allow you to group and load related application features on-demand, which helps decrease the application startup time.  

No need to load everything at once, you should only load what the user expects to see when the app first appears. 

Lazy loaded feature modules only get loaded when a user navigates to the feature’s route. 

For more information, see [Feature modules](https://angular.io/guide/feature-modules) and [Lazy-loading feature modules](https://angular.io/guide/lazy-loading-ngmodules).

## Add A Feature Module

From the terminal window, run the following command line statement:

```
ng generate module dashboard --route dashboard --module app.module
```

You should see something like the following:

![image.png](/.attachments/image-f0b4a08e-39fd-4335-baae-b04aef57e0b9.png)

In the VS Code files area, the following folder and files sare created for you:

![image.png](/.attachments/image-ef968f46-8aea-46f7-98fe-fd50088395dc.png)

Run the app.

```
npm run start
```

Click on the **Dashboard** link in the page header.

The app should navigate to the **Dashboard** view.

You should see something like the following:

![image.png](/.attachments/image-cca6c6c1-963e-4a5c-8094-a2df1e0c4011.png)

Open the **dashboard.module.ts** file.

It should contain the following:

``` typescript
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';

import { DashboardRoutingModule } from './dashboard-routing.module';
import { DashboardComponent } from './dashboard.component';


@NgModule({
  declarations: [DashboardComponent],
  imports: [
    CommonModule,
    DashboardRoutingModule
  ]
})
export class DashboardModule { }
```

Open the **dashboard-routing.module.ts** file.

It should contain the following, which you should reformat as shown:

``` typescript
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';

import { DashboardComponent } from './dashboard.component';

const routes: Routes = [
  {
    path: '', component: DashboardComponent
  }
];

@NgModule({
  imports: [RouterModule.forChild(routes)],
  exports: [RouterModule]
})
export class DashboardRoutingModule { }
```

Open the **app-routing.module.ts** file.

You should see the following new route entry:

![image.png](/.attachments/image-75d83512-07b9-45e9-abea-fb0c07466284.png)

You should reformat this entry and move it to the desired location. Add the data:title as shown:

![image.png](/.attachments/image-8b6405f3-cefe-4719-8a9e-8359b71d26ad.png)

``` typescript
  {
    path: 'dashboard',
    loadChildren: () => import('./dashboard/dashboard.module').then(m => m.DashboardModule),
    data: {
      title: 'Dashboard'
    }
  },
```





