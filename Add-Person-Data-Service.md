# Summary

Now we'll add our first data service.

## Create Odata Response Model

In the src\app\models folder, create a new file called:

```
odata-response.model.ts
```

Copy the following code into the **odata-response.model.ts** file:

``` typescript
export class OdataResponse {
    count: number;
    value: any[];
    constructor() {
        this.count = 0;
        this.value = [];
    }
}
```

This file is used to map all incoming OData query responses.

## Create Person Model

In the src\app\modelsfolder, create a new file called:

```
person.model.ts
```

Copy the following code into the **person.model.ts** file:

``` typescript
export class Person {
    Id: number;
    Name: string;
    Email1: string;
    Email2: string;
    Phone1: string;
    Phone2: string;
    Street1: string;
    City1: string;
    StateProvince1: string;
    PostalCode1: string;
    Street2: string;
    City2: string;
    StateProvince2: string;
    PostalCode2: string;
}
```

## Create Person Data Service

In the VS Code terminal windows, run the following command:

```
ng g s services\person
```

You should see output similar to the following:

![image.png](/.attachments/image-85a78ebb-b41e-443e-aef6-526ae26f8ca7.png)

The Angular CLI creates the **PersonService** files in the src\app\services folder.

![image.png](/.attachments/image-9d4e97f2-67ea-4099-abfb-0f9f9b4efdb8.png)

Open the **services.module.ts** file, and add the **PersonService** code to it as shown below:

``` typescript
import { NgModule, ModuleWithProviders, Optional, SkipSelf, APP_INITIALIZER } from '@angular/core';
import { CommonModule } from '@angular/common';
import { TitleService } from './title.service';
import { AppSettingsService } from './app-settings.service';
import { LoggerService } from './logger.service';
import { ErrorService } from './error.service';
import { PersonService } from './person.service';

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
        ErrorService,
        PersonService
      ]
    };
  }
}
```

Open the **person.service.ts** file, and replace the code with the following:

``` typescript
import { Injectable } from '@angular/core';
import { HttpClient, HttpErrorResponse } from '@angular/common/http';
import { map, catchError } from 'rxjs/operators';
import * as lodash from 'lodash';
import { debug, RxJsLoggingLevel } from './debug';
import { ErrorService } from './error.service';
import { AppSettingsService } from './app-settings.service';
import { LoggerService } from './logger.service';
import { Person } from '../models/person.model';
import { OdataResponse } from '../models/odata-response.model';

@Injectable({
  providedIn: 'root'
})
export class PersonService {

  baseUrl: string;

  constructor(
    private http: HttpClient,
    private loggerService: LoggerService,
    private errorService: ErrorService,
    private appSettingsService: AppSettingsService
  ) {
    this.baseUrl = this.appSettingsService.AppSettings.baseApiUrl;
  }

  countPeople(query: string): Promise<number> {
    const url = `${this.baseUrl}/odata/people/$count${query}`;
    return this.http.get<number>(url).pipe(
      debug(RxJsLoggingLevel.DEBUG, 'PersonService: countPeople'),
      catchError((error) => this.handleError<number>(error, 'Unable to count people.', 'countPeople', 0)),
      map((count: any) => {
        return count;
      })
    ).toPromise();
  }

  fetchPeople(query: string): Promise<OdataResponse> {
    const url = `${this.baseUrl}/odata/people${query}`;
    return this.http.get<OdataResponse>(url).pipe(
      debug(RxJsLoggingLevel.DEBUG, 'PersonService: fetchPeople'),
      catchError((error) => this.handleError<OdataResponse>(error, 'Unable to retrieve people.', 'fetchPeople', new OdataResponse())),
      map((response: any) => {
        const count = response['@odata.count'];
        const people: Person[] = [];
        lodash.forEach(response.value, (value, key) => {
          if (response.value.hasOwnProperty(key)) {
            people.push({ ...value });
          }
        });
        const result = {
          count,
          value: people
        } as OdataResponse;
        return result;
      })
    ).toPromise();
  }

  getPerson(id: number): Promise<Person> {
    const url = `${this.baseUrl}/odata/people?$filter=Id%20eq%20${id}`;
    return this.http.get<Person>(url).pipe(
      debug(RxJsLoggingLevel.DEBUG, 'PersonService: getPerson'),
      catchError((error) => this.handleError<Person>(error, 'Unable to retrieve person.', 'getPerson')),
      map((response: any) => {
        let person: Person;
        if (response?.value.length > 0) {
          person = response.value[0] as Person;
        }
        return person;
      })
    ).toPromise();
  }

  handleError<T>(error: HttpErrorResponse, message: string, methodName: string, result?: T) {
    const source = 'Person Service';
    return this.errorService.handleError<T>(error, source, message, methodName, result);
  }
}
```

In the terminal window, verify there are no issues reported:

![image.png](/.attachments/image-a5381f82-8d39-4693-bca2-57286c742a8a.png)

Run the app.

```
npm run start
```

You have now added the Person data Service.