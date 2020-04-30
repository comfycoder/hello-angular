# Summary

You should be able to selectively turn on different levels of logging output to the browser console.

Add a new **LoggerService** class to the project.

```
ng g s services\logger
```

You should see output similar to the following:

![image.png](/.attachments/image-131ad5a0-e0df-4be7-beaa-cd344beb65dd.png)

The Angular CLI creates the **LoggerService** files in the services folder.

![image.png](/.attachments/image-8e5de004-ff59-4080-bb37-c3a798ffc278.png)

Open the **services.module.ts** file, and add the **LoggerService** to it as shown below:

![image.png](/.attachments/image-e9e5c022-ad6d-4027-8f03-ca277a9f85b4.png)

``` typescript
import { NgModule, ModuleWithProviders, Optional, SkipSelf, APP_INITIALIZER } from '@angular/core';
import { CommonModule } from '@angular/common';
import { TitleService } from './title.service';
import { AppSettingsService } from './app-settings.service';
import { LoggerService } from './logger.service';

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
        LoggerService
      ]
    };
  }
}
```

Open the **logger.service.ts** file, and replace the code with the following:

``` typescript
import { Injectable } from '@angular/core';

export enum LogLevel {
  Trace,
  Debug,
  Information,
  Warning,
  Error
}

@Injectable({
  providedIn: 'root'
})
export class LoggerService {

  // tslint:disable-next-line: variable-name
  private _logLevel: LogLevel = LogLevel.Information;

  public get logLevel(): LogLevel {
    return this._logLevel;
  }

  constructor() { }

  public setLogLevel(value: LogLevel) {
    this._logLevel = value;
  }

  log(logLevel: LogLevel, message?: string, ...optionalParams: any[]) {
    optionalParams = optionalParams?.length ? optionalParams : undefined;
    if (logLevel === LogLevel.Trace && this.logLevel <= LogLevel.Trace) {
      console.log(`TRACE: ${message}`, optionalParams);
    } else if (logLevel === LogLevel.Debug && this.logLevel <= LogLevel.Debug) {
      console.log(`DEBUG: ${message}`, optionalParams);
    } else if (logLevel === LogLevel.Information && this.logLevel <= LogLevel.Information) {
      console.log(`INFO: ${message}`, optionalParams);
    } else if (logLevel === LogLevel.Warning && this.logLevel <= LogLevel.Warning) {
      console.warn(`WARN: ${message}`, optionalParams);
    } else if (logLevel === LogLevel.Error && this.logLevel <= LogLevel.Error) {
      console.error(`ERROR: ${message}`, optionalParams);
    }
  }

  trace(message?: any, ...optionalParams: any[]) {
    optionalParams = optionalParams?.length ? optionalParams : undefined;
    if (this.logLevel <= LogLevel.Trace) {
      console.log(`TRACE: ${message}`, optionalParams);
    }
  }

  debug(message?: any, ...optionalParams: any[]) {
    optionalParams = optionalParams?.length ? optionalParams : undefined;
    if (this.logLevel <= LogLevel.Debug) {
      console.log(`DEBUG: ${message}`, optionalParams);
    }
  }

  info(message?: any, ...optionalParams: any[]) {
    optionalParams = optionalParams?.length ? optionalParams : undefined;
    if (this.logLevel <= LogLevel.Information) {
      console.log(`INFO: ${message}`, optionalParams);
    }
  }

  warn(message?: any, ...optionalParams: any[]) {
    optionalParams = optionalParams?.length ? optionalParams : undefined;
    if (this.logLevel <= LogLevel.Warning) {
      console.warn(`WARN: ${message}`, optionalParams);
    }
  }

  error(message?: any, ...optionalParams: any[]) {
    optionalParams = optionalParams?.length ? optionalParams : undefined;
    if (this.logLevel <= LogLevel.Error) {
      console.error(`ERROR: ${message}`, optionalParams);
    }
  }
}
```

# Logging Verbosity Levels

When adding logging statements to your application, you must specify a LogLevel. The LogLevel allows you to control the verbosity of the logging output from your application.

## Log Levels

``` typescript
export enum LogLevel {
  Trace,
  Debug,
  Information,
  Warning,
  Error
}
```

Five levels of logging verbosity, ordered by increasing importance or severity:

**Trace**
Used for the most detailed log messages, typically only valuable to a developer debugging an issue. These messages may contain sensitive application data and so should not be enabled in a production environment. Disabled by default. 

Example: 
```
Credentials: {"User":"someuser", "Password":"P@ssword"}
```

**Debug**
These messages have short-term usefulness during development. They contain information that may be useful for debugging but have no long-term value. This is the default most verbose level of logging. Disabled by default. 

Example: 
```
Entering method Configure with flag set to true
```

**Information**
These messages are used to track the general flow of the application. These logs should have some long term value, as opposed to Verbose level messages, which do not. 

Example: 
```
Request received for path /foo
```

**Warning**
The Warning level should be used for abnormal or unexpected events in the application flow. These may include errors or other conditions that do not cause the application to stop, but which may need to be investigated in the future. Handled exceptions are commonplace to use the Warning log level. 

Examples: 
```
Login failed for IP 127.0.0.1
```
or 
```
FileNotFoundException for file foo.txt
```

**Error**
An error should be logged when the current flow of the application must stop due to some failure, such as an exception that cannot be handled or recovered from. These messages should indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure. 

Example: 
```
Cannot insert record due to duplicate key violation
```

In the terminal windows, verify there are no issues reported:

![image.png](/.attachments/image-a5381f82-8d39-4693-bca2-57286c742a8a.png)

Run the app.

```
npm run start
```

You have now added the Logger service.

