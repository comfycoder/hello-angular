# Summary

Create an Error service to handle errors generated in data service classes.

Add a new **ErrorService** class to the project.

```
ng g s services\error
```

You should see output similar to the following:

![image.png](/.attachments/image-99827d71-dc8f-4951-9b95-f1d2c42e52e6.png)

The Angular CLI creates the **LoggerService** files in the services folder.

![image.png](/.attachments/image-bf3cba42-07fb-404f-893c-f866f8f37797.png)

Open the **services.module.ts** file, and add the **ErrorService** code to it as shown below:

![image.png](/.attachments/image-f211491b-ec85-47a3-adea-ddc2ddc4c614.png)

``` typescript
import { NgModule, ModuleWithProviders, Optional, SkipSelf, APP_INITIALIZER } from '@angular/core';
import { CommonModule } from '@angular/common';
import { TitleService } from './title.service';
import { AppSettingsService } from './app-settings.service';
import { LoggerService } from './logger.service';
import { ErrorService } from './error.service';

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
        },
        LoggerService,
        ErrorService
      ]
    };
  }
}
```

Open the **error.service.ts** file, and replace the code with the following:

``` typescript
import { Injectable } from '@angular/core';
import { HttpErrorResponse } from '@angular/common/http';
import { ToastrService } from 'ngx-toastr';
import { of, throwError } from 'rxjs';
import { LoggerService } from './logger.service';

@Injectable({
  providedIn: 'root'
})
export class ErrorService {

  constructor(
    private loggerService: LoggerService,
    private toastrService: ToastrService
  ) { }

  handleError<T>(error: HttpErrorResponse, source: string, message = 'API Call Failed', methodName: string, result?: T) {
    let errorMessage = `${source}: Unknown error`;
    if (error.error instanceof ErrorEvent) {
      // Client-side errors
      errorMessage = `${source}: ${methodName}: ${error.error.message}`;
    } else {
      // Server-side errors
      errorMessage = `${source}: ${methodName}: Error Code: ${error.status}:\nMessage: ${error.message}`;
    }
    this.loggerService.error(errorMessage, error);
    if (result) {
      this.toastrService.error(message, source);
      return of(result as T);
    } else {
      return throwError(errorMessage);
    }
  }
}
```

Note that this class also depends on the **ngx-toastr** service.

From the VS Code terminal window, execute the following:

```
yarn add ngx-toastr
```

In the **package.json** file, you should see an entry similar to the following:

![image.png](/.attachments/image-07eb513f-96d2-43f7-8389-dffb46cb9151.png)

In the VS Code terminal windows, you should see output similar to the following:

![image.png](/.attachments/image-69e1b868-1f25-4304-99b8-b4855504c699.png)

Open the app.module.ts file, and add the BrowserAnimationsModule and ToasterModule as shown in the following:

![image.png](/.attachments/image-503e2eb4-0a23-4cb3-841e-a53f151ca660.png)

``` typescript
import { BrowserModule, Title } from '@angular/platform-browser';
import { BrowserAnimationsModule } from '@angular/platform-browser/animations';
import { NgModule } from '@angular/core';
import { HttpClientModule, HttpClient } from '@angular/common/http';

// ngx-translate modules
import { TranslateModule, TranslateLoader } from '@ngx-translate/core';
import { TranslateHttpLoader } from '@ngx-translate/http-loader';

// Import ngx-toastr library module
import { ToastrModule } from 'ngx-toastr';

// AoT requires an exported function for factories
export function HttpLoaderFactory(httpClient: HttpClient) {
  return new TranslateHttpLoader(httpClient);
}

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
    BrowserAnimationsModule,
    AppRoutingModule,
    HttpClientModule,
    ToastrModule.forRoot({
      timeOut: 10000,
      positionClass: 'toast-bottom-right',
      preventDuplicates: true,
    }),
    TranslateModule.forRoot({
      loader: {
        provide: TranslateLoader,
        useFactory: HttpLoaderFactory,
        deps: [HttpClient]
      }
    }),
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

Open the **angular.json** file, and add the following CSS stylesheet reference:

![image.png](/.attachments/image-2c3e36fe-7cf4-4280-b91a-41199d457cc2.png)

``` json
"node_modules/ngx-toastr/toastr.css",
```

In the terminal window, verify there are no issues reported:

![image.png](/.attachments/image-a5381f82-8d39-4693-bca2-57286c742a8a.png)

Run the app.

```
npm run start
```

You have now added the Error service.

