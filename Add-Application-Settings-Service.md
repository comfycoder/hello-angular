# Summary

Angular applications typically communicate with backend services. These services usually have different base URLs for each server environment (development, demo, production, etc.) This section shows you how to create a service used to read custom environment configuration files into your Angular application.

Add a new **AppSettingsService** class to the project.

```
ng g s services\app-settings
```

You should see output similar to the following:

![image.png](/.attachments/image-eba9c54d-82a7-421b-993d-b20ed0cfabc0.png)

The Angular CLI creates the **AppSettingsService** in the services folder.

![image.png](/.attachments/image-04ba3610-6de8-49d9-99c1-6c81b60be997.png)

Open the **services.module.ts** file, and add the **AppSettingsService** to it as shown below:

![image.png](/.attachments/image-93baa91c-72b9-4dd8-994a-42c0e566da0f.png)

``` typescript
import { NgModule, ModuleWithProviders, Optional, SkipSelf, APP_INITIALIZER } from '@angular/core';
import { CommonModule } from '@angular/common';
import { TitleService } from './title.service';
import { AppSettingsService } from './app-settings.service';

export function appSettingServiceFactory(appSettingsService: AppSettingsService) {
  return () => appSettingsService.load();
}

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
        TitleService,
        AppSettingsService,
        {
          provide: APP_INITIALIZER,
          deps: [AppSettingsService],
          useFactory: appSettingServiceFactory,
          multi: true
        }
      ]
    };
  }
}
```

Note the use of the APP_INITIALIZER pattern, which is an injection token that allows you to provide one or more initialization functions. These functions are injected at application startup and executed during app initialization.

Open the **app-settings.service.ts** file, and replace the code with the following:

``` typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { environment } from '../../environments/environment';
import { AppSettings } from '../models/app-settings.model';

@Injectable({
  providedIn: 'root'
})
export class AppSettingsService {

  private appSettingsUrl: string;

  // tslint:disable-next-line: variable-name
  private _appSettings: AppSettings;

  constructor(
    private http: HttpClient
  ) {
    this.appSettingsUrl = `assets/${environment.configFile}`;
    // console.log(this.appSettingsUrl);
  }

  // This is the method you want to call at bootstrap
  // Important: It should return a Promise
  load(): Promise<any> {

    this._appSettings = null;

    return this.http
      .get(this.appSettingsUrl)
      .toPromise()
      .then((data: any) => {
        this._appSettings = data as AppSettings;
      })
      .catch((err: any) => Promise.resolve());
  }

  get AppSettings(): AppSettings {
    return (this._appSettings) ? this._appSettings : new AppSettings();
  }

  set AppSettings(appSettings: AppSettings) {
    this._appSettings = appSettings;
  }
}
```

In VS Code in the **src\app** folder, create a **models** folder.

In the **models** folder, create a new file called **app-settings.model.ts**.

![image.png](/.attachments/image-2e3de30b-d0d2-4cff-a195-b74923b4872f.png)

Add the following code to the **app-settings.model.ts** file:

``` typescript
export class AppSettings {
    baseApiUrl: string;
}
```

In VS Code, open the src\assets\environments folder:

![image.png](/.attachments/image-d92d807c-28ca-4973-8f59-f1ca1e68a488.png)

Replace the code in the **environment.prod.ts** file with the following:

``` typescript
export const environment = {
  production: true,
  configFile: 'app-settings.json'
};
```

Replace the code in the **environment.ts** file with the following:

``` typescript
// This file can be replaced during build by using the `fileReplacements` array.
// `ng build --prod` replaces `environment.ts` with `environment.prod.ts`.
// The list of file replacements can be found in `angular.json`.

export const environment = {
  production: false,
  configFile: 'app-settings.local.json'
};

/*
 * For easier debugging in development mode, you can import the following file
 * to ignore zone related error stack frames such as `zone.run`, `zoneDelegate.invokeTask`.
 *
 * This import should be commented out in production mode because it will have a negative impact
 * on performance if an error is thrown.
 */
// import 'zone.js/dist/zone-error';  // Included with Angular CLI
```

The new **configFile** variable that we added to these two files tells the **AppSettingsService** which **appsettings** file to load, depending on the environment for which the app was built.

In the src\assets folder, create the app-settings.json file. This is the production release app settings file.

Add the following code to this file:

``` json
{
    "baseApiUrl": "__BASE_API_URL__"
}
```

In the src\assets folder, create the app-settings.local.json file. This is the local development app settings file.

Add the following code to this file:

``` json
{
    "baseApiUrl": "https://as-nl-test-api.azurewebsites.net"
}
```

This sets the configuration variable for the local developer's environment to specify the location of the development APIs.

Open the **app.component.ts** file and edit the code as shown below:

![image.png](/.attachments/image-11f76749-db02-49b4-bb4c-216cbedc537f.png)

``` typescript
import { Component, OnInit } from '@angular/core';
import { TitleService } from './services/title.service';
import { AppSettingsService } from './services/app-settings.service';
import { AppSettings } from './models/app-settings.model';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})
export class AppComponent implements OnInit {

  title = 'My App';
  appSettings: AppSettings;

  constructor(
    private titleService: TitleService,
    private appSettingsService: AppSettingsService
  ) { }

  ngOnInit(): void {
    this.titleService.init(this.title);
    this.appSettings = this.appSettingsService.AppSettings;
  }
}
```

In the example above, we used dependency injection to inject the **AppSettingsService** into the component constructor.

``` typescript
constructor(
...
  private appSettingsService: AppSettingsService
) { }
```

Note that in Angular, the injected dependencies are automatically created as class-level variables, so you do not need to declare a separate variable to use them.

We added a new class variable to hold the app settings values that were read from the file.

``` typescript
appSettings: AppSettings;
```

We also added the **ngOnInit()** lifecycle event handler to handle component initialization tasks. In this case, it initializes the **AppSettingsService** by setting the application name.

``` typescript
ngOnInit(): void {
  ...
  this.appSettings = this.appSettingsService.AppSettings;
}
```

Open the **app.module.ts** file, and edit the code as shown in the following:

![image.png](/.attachments/image-780bdf46-4b58-461b-8561-d379968d3d33.png)

``` typescript
import { BrowserModule, Title } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { HttpClientModule } from '@angular/common/http';

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
    HttpClientModule,
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

Note that we needed to add the Angular HttpClientModeule for use throughout the app.

Run the app.

```
npm run start
```

Open up the browser developer tools (usually press F12) to the **Sources** tab.

In the **Page** tree view, drill down until you find the file and select it.

![image.png](/.attachments/image-2ac233ba-268c-4567-af6c-55a2a4afe0a3.png)

In the source code view, set a breakpoint on the indicated line:

![image.png](/.attachments/image-0579386a-9ed6-40c8-9453-5b3231c66028.png)

Press F5 to refresh the browser.

The breakpoint should be hit in the code and you should be able to see the value retrieved from the app-settings.json file:

![image.png](/.attachments/image-21e5cc90-4f01-4776-9a0d-d20e5c1a93c6.png)

In the browser developers, switch to the **Network** tab.

Press F5 to refresh the browser.

You should be able to see the appsettings.local.json file in the list of files downloaded to the browser, and you can preview the contents of the file, as shown below:

![image.png](/.attachments/image-efe07487-f07a-4c7d-9584-ca7dc8c8ad59.png)

You now have a way to read the application settings into any component, services, etc. throughout your app by injecting an AppSettingsService class instance.
















