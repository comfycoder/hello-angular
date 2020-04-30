# Summary

# Create A Store Module 

From the terminal window, create a module for globally available in-memory data stores: 

```
ng g m stores
```

In VS Code, navigate to the new **stores.module.ts file** in the **src/app/stores** folder.

![image.png](/.attachments/image-4d65e904-bbd9-4c8c-9b37-05df04e45d17.png)

Edit the stores.module.ts and replace code with the following:

``` typescript
import { NgModule, Optional, SkipSelf, ModuleWithProviders } from '@angular/core';
import { CommonModule } from '@angular/common';

@NgModule({
  declarations: [],
  imports: [
    CommonModule
  ]
})
export class StoresModule {

  constructor(@Optional() @SkipSelf() parentModule: StoresModule) {
    if (parentModule) {
      throw new Error(
        'StoresModule is already loaded. Import it in the AppModule only');
    }
  }

  static forRoot(): ModuleWithProviders<StoresModule> {
    return {
      ngModule: StoresModule,
      providers: [

      ]
    };
  }
}
```

In the src\app\stores folder, create a new file called:

```
abstract-base.store.ts
```

Copy the following code into this file:

``` typescript
import { Observable, BehaviorSubject } from 'rxjs';
import { pluck, distinctUntilChanged } from 'rxjs/operators';

export class Store<T> {

    public state$: Observable<T>;

    // tslint:disable-next-line: variable-name
    private _state$: BehaviorSubject<T>;

    protected constructor(initialState: T) {
        this._state$ = new BehaviorSubject<T>(initialState);
        this.state$ = this._state$.asObservable().pipe(distinctUntilChanged());
    }

    public getState(): T {
        return this._state$.getValue();
    }

    public setNextState(nextState: T): void {
        this._state$.next(nextState);
    }

    public setState(name: string, state: any) {
        // console.log('setState: ' + name, state);
        this._state$.next({
            ...this.getState(),
            [name]: state
        });
    }

    select<TProp>(name: string): Observable<TProp> {
        return this.state$.pipe(pluck(name)) as Observable<TProp>;
    }
}
```

# Add StoresModule to the main app.module.ts file

In VS Code, navigate to the new **app.module.ts** file in the **src/app** folder.

Edit the module as shown in the following:

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
import { StoresModule } from './stores/stores.module';
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
    ComponentsModule.forRoot(),
    StoresModule.forRoot()
  ],
  providers: [
    Title
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

In the terminal window, verify there are no issues reported:

![image.png](/.attachments/image-a5381f82-8d39-4693-bca2-57286c742a8a.png)

Run the app.

```
npm run start
```

You have now added the StoresModule from which to share in-memory data stores.

