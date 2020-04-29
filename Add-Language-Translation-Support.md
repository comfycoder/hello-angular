# Summary

This section covers how to use the **ngx-translate** library to support language translation of text displayed on the website.

## What is ngx-translate?
NGX-Translate is an internationalization library for Angular. It lets you define translations for your content in different languages and switch between them easily.

It gives you access to a service, a directive, and a pipe to handle any dynamic or static content.

NGX-Translate is also extremely modular. It is written in a way that makes it really easy to replace any part with a custom implementation in case the existing one doesn't fit your needs.

In VS Code, open a terminal window and run the following commands:

```
yarn add @ngx-translate/core
yarn add @ngx-translate/http-loader
```

You should see something like the following added to the **package.json** file:

![image.png](/.attachments/image-9402c950-9e6d-4a8e-a806-6a4e980f85ba.png)

Open the **app.module.ts** file and make the following changes:

![image.png](/.attachments/image-4cae9e77-85dd-4bd4-ba4a-4fe2e84d906d.png)

``` typescript
import { BrowserModule, Title } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { HttpClientModule, HttpClient } from '@angular/common/http';

// ngx-translate modules
import { TranslateModule, TranslateLoader } from '@ngx-translate/core';
import { TranslateHttpLoader } from '@ngx-translate/http-loader';

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
    AppRoutingModule,
    HttpClientModule,
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

This enables you to use the **TranslateHttpLoader** to load translations from the language translation files you place in the **src\assets\i18n** folder.

Open the **app.component.ts** file and make the following changes:

![image.png](/.attachments/image-02d0acf1-16a7-4e0c-a4bf-9c475d144d44.png)

``` typescript
import { Component, OnInit } from '@angular/core';
import { TitleService } from './services/title.service';
import { AppSettingsService } from './services/app-settings.service';
import { AppSettings } from './models/app-settings.model';

import { TranslateService } from '@ngx-translate/core';

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
    private appSettingsService: AppSettingsService,
    public translate: TranslateService
  ) {
    // Add new languages to the list
    translate.addLangs(['en', 'fr']);
    // Sets the language used as a fallback when a
    // translation isn't found in the current language
    translate.setDefaultLang('en');
    // Gets the current browser language if available, or undefined otherwise
    const browserLang = translate.getBrowserLang();
    // The language to use, if the language isn't available,
    // it will use the current loader to get them
    translate.use(browserLang.match(/en|fr/) ? browserLang : 'en');
  }

  ngOnInit(): void {
    this.titleService.init(this.title);
    this.appSettings = this.appSettingsService.AppSettings;
    this.translate.use('en');
  }
}
```

Open the **components.module.ts** file and make the following changes:

![image.png](/.attachments/image-b26c2a47-0f14-4958-b4d1-d94bc1d6bf0e.png)

``` typescript
import { NgModule, ModuleWithProviders } from '@angular/core';
import { CommonModule } from '@angular/common';
import { RouterModule } from '@angular/router';
import { FormsModule, ReactiveFormsModule } from '@angular/forms';

import { AgGridModule } from '@ag-grid-community/angular';
import { TranslateModule } from '@ngx-translate/core';

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
    ReactiveFormsModule,
    TranslateModule,
    AgGridModule.withComponents([])
  ],
  exports: [
    CommonModule,
    RouterModule,
    FormsModule,
    ReactiveFormsModule,
    TranslateModule,
    AgGridModule,
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

We added the **TranslateModule** to the **imports** and **exports** blocks, and we add an import statement to include the TranslatModule in this module file. This makes the **TranslateModule** visible to all of the components in the shared components module.

In the src\assets folder, create the i18n folder.

In the i18n folder, add a language translation JSON file for each language your website needs to support. 

e.g.,

![image.png](/.attachments/image-01873841-0c81-46f4-9d94-ba947ea049ee.png)

In each JSON file, there is a key/value pair for each text item translation. All of the keys must match the keys in the mast language JSON file (i.e., en.json). These are used to look up the appropriate translation when another language is specified.

In this particular site, the en.json file is the master (default) language.

### Sample en.json file:

``` json
{
	"Dashboard": "Dashboard",
	"Error": "Error",
	"Home": "Home",
	"My App": "My App",
	"Not Found": "Not Found",
	"T_ERROR_PAGE_1_TEXT": "We're sorry, but something went wrong.",
	"T_ERROR_PAGE_2_TEXT": "Please try your last action again.",
	"T_ERROR_PAGE_3_TEXT": "If the problem persists, please contact support for assistance.",
	"T_HOME_PAGE_1_HEAD": "Welcome to my app!",
	"T_HOME_PAGE_1_TEXT": "We have a lot of fun and educational activities planned for you.",
	"T_HOME_PAGE_2_TEXT": "We hope you enjoy your time here at my app.",
	"T_NOT_FOUND_PAGE_1_TEXT": "We're sorry, but we are unable to provide the location you requested.",
	"T_NOT_FOUND_PAGE_2_TEXT": "Please submit a request for a different location."
}
```

### Sample de.json file:

``` json
{
	"Dashboard": "Instrumententafel",
	"Error": "Error",
	"Home": "Zuhause",
	"My App": "Meine App",
	"Not Found": "Nicht gefunden",
	"T_ERROR_PAGE_1_TEXT": "Es tut uns leid, aber etwas is schief gelaufen.",
	"T_ERROR_PAGE_2_TEXT": "Bitte versuchen Sie Ihre letzte Aktion erneut.",
	"T_ERROR_PAGE_3_TEXT": "Wenn das Problem weiterhin besteht, wenden Sie sich an den Support.",
	"T_HOME_PAGE_1_HEAD": "Willkommen in meiner App!",
	"T_HOME_PAGE_1_TEXT": "Wir haben viel Spaß und lehrreiche Aktivitäten für Sie geplant.",
	"T_HOME_PAGE_2_TEXT": "Wir hoffen, Sie genießen Ihre Zeit hier in meiner App.",
	"T_NOT_FOUND_PAGE_1_TEXT": "Es tut uns leid, aber wir können den von Ihnen gewünschten Ort nicht angeben.",
	"T_NOT_FOUND_PAGE_2_TEXT": "Bitte senden Sie eine Anfrage für einen anderen Ort."
}
```

In HTML, use the following pipe pattern to translate the text for and element.

### Sample HTML with translations:

``` html
<h1>
    {{ 'T_HOME_PAGE_1_HEAD' | translate }}
</h1>
<p>
    {{ 'T_HOME_PAGE_1_TEXT' | translate }}
</p>
<p>
    {{ 'T_HOME_PAGE_2_TEXT' | translate }}
</p>
<p>
    <button type="button" class="btn btn-primary" [routerLink]="['/error']">{{ 'Error' | translate }}</button>
</p>
<p>
    <button type="button" class="btn btn-primary" [routerLink]="['/not-found']">{{ 'Not Found' | translate}}</button>
</p>
```

Text can also be translated programmatically in code by injecting the TranslateService into a component or class and passing the text translation key into the use method.

``` typescript
const caption = this.translate.use('Submit');
```

### Sample ag-grid cell renderer:

``` typescript
import {Component, NgZone, OnInit, OnDestroy} from '@angular/core';
import {ICellRendererAngularComp} from '@ag-grid-community/angular';
import { Router } from '@angular/router';
import { TranslateService } from '@ngx-translate/core';
import { Subscription } from 'rxjs';
import { ICellRendererParams } from '@ag-grid-community/core';

@Component({
    selector: 'app-order-action-renderer',
    template: `{{ caption }}`
})
// tslint:disable-next-line: component-class-suffix
export class TranslateTextRenderer implements ICellRendererAngularComp, OnDestroy {
    public params: any;
    public caption: string;

    private translateSub: Subscription;

    constructor(
        private ngZone: NgZone,
        private router: Router,
        private translateService: TranslateService
    ) {}

    agInit(params: ICellRendererParams): void {
        this.params = params;
        if (params.valueFormatted) {
            this.caption = this.translateService.instant(params.valueFormatted);
        } else {
            if (params.value !== undefined) {
                this.caption = this.translateService.instant(params.value);
            }
        }
    }
    ngOnDestroy(): void {
        if (this.translateSub) {
            this.translateSub.unsubscribe();
        }
    }

    refresh(): boolean {
        return false;
    }
}
```




