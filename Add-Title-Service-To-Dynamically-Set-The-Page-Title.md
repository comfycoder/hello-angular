# Summary 

Your app should be able to make the browser title bar say whatever you want it to say. 

The Angular **Title** service is a simple class that provides an API for getting and setting the current HTML document title.

Add a new **TitleService** class to the project.

```
ng g s services\title
```

You should see output similar to the following:

![image.png](/.attachments/image-f6cd4be6-84d0-4e89-94a6-de896b4a808c.png)

The Angular CLI creates the **TitleService** in the services folder.

![image.png](/.attachments/image-eb0c966f-1f52-458e-b3fc-3896645c675c.png)

Open the **services.module.ts** file, and add the **TitleService** to it as shown below:

![image.png](/.attachments/image-04b2d93f-1ba5-4756-a5ec-6051b9cb556c.png)

``` typescript
import { NgModule, ModuleWithProviders, Optional, SkipSelf } from '@angular/core';
import { CommonModule } from '@angular/common';
import { TitleService } from './title.service';

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
        TitleService
      ]
    };
  }
}
```

Open the **title.service.ts** file, and replace the code with the following:

``` typescript
import { Injectable } from '@angular/core';

import { map, filter } from 'rxjs/operators';

import { Router, NavigationEnd, ActivatedRoute } from '@angular/router';
import { Title } from '@angular/platform-browser';

const SEPARATOR = ' > ';

@Injectable({
  providedIn: 'root'
})
export class TitleService {

  appName: string;
  pathString: string;

  static ucFirst(text: string) {
    if (!text) { return text; }
    return text.charAt(0).toUpperCase() + text.slice(1);
  }

  constructor(
    private router: Router,
    private activatedRoute: ActivatedRoute,
    private titleService: Title,
  ) { }

  init(appName: string) {
    this.appName = appName;
    this.router.events.pipe(
      filter((event) => event instanceof NavigationEnd),
      map(() => {
        let route = this.activatedRoute;
        while (route.firstChild) { route = route.firstChild; }
        return route;
      }),
      filter((route) => route.outlet === 'primary'),
      map((route) => route.snapshot),
      map((snapshot) => {
        if (snapshot.data.title) {
          if (snapshot.paramMap.get('id') !== null) {
            return snapshot.data.title + SEPARATOR + snapshot.paramMap.get('id');
          }
          return snapshot.data.title;
        } else {
          // If not, we do a little magic on the url to create an approximation
          return this.router.url.split('/').reduce((acc, frag) => {
            if (acc && frag) { acc += SEPARATOR; }
            return acc + TitleService.ucFirst(frag);
          });
        }
      }))
      .subscribe((pathString) => {
        this.pathString = pathString;
        this.titleService.setTitle(`${this.pathString} - ${this.appName}`);
      });
  }

  setAppName(appName: string) {
    this.appName = appName;
    this.titleService.setTitle(`${this.pathString} - ${this.appName}`);
  }
}
```

Open the **app.component.ts** file and edit the code as shown below:

![image.png](/.attachments/image-f905d468-e240-448b-bb44-eeba9e4cc57e.png)

``` typescript
import { Component, OnInit } from '@angular/core';
import { TitleService } from './services/title.service';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})
export class AppComponent implements OnInit {

  title = 'My App';

  constructor(
    private titleService: TitleService
  ) { }

  ngOnInit(): void {
    this.titleService.setAppName(this.title);
  }
}
```

In the example above, we used dependency injection to inject the **TitleService** into the component constructor.

``` typescript
constructor(
    private titleService: TitleService
  ) { }
```

Note that in Angular, the injected dependencies are automatically created as class-level variables, so you do not need to declare a separate variable to use them.

We also added the **ngOnInit()** lifecycle event handler to handle component initialization tasks. In this case, it initializes the **TitleService** by setting the application name.

``` typescript
  ngOnInit(): void {
    this.titleService.setAppName(this.title);
  }
```

We also added **OnInit** to the class declaration to wire up the ngOnInit event handler.

``` typescript
export class AppComponent implements OnInit {
...
}
```

Open the **app.module.ts** file, and edit the code as shown in the following:

![image.png](/.attachments/image-482a2470-4177-4125-a977-988741a36385.png)

``` typescript
import { BrowserModule, Title } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { ServicesModule } from './services/services.module';
import { ComponentsModule } from './components/components.module';
import { HomeComponent } from './home/home.component';
import { ErrorComponent } from './error/error.component';
import { NotFoundComponent } from './not-found/not-found.component';

@NgModule({
  declarations: [
    AppComponent,
    HomeComponent,
    ErrorComponent,
    NotFoundComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    ServicesModule.forRoot(),
    ComponentsModule.forRoot()
  ],
  providers: [
    Title
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

We have now added the Angular **Title** service class to the application root scope by registering it in the **providers** block. Our custom **TitleService** can now make use of it.

Run the app.

```
npm run start
```

The app launches showing the home page title in the browser tab:

![image.png](/.attachments/image-c40f340c-08d9-4802-b75e-a8348ea5d957.png)

Navigate to the dashboard page.

You should see the dashboard page title in the browser tab:

![image.png](/.attachments/image-0b2f7769-4892-4fab-a754-5c5f7ca3e97b.png)





