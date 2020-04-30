# Summary

Add a function that works with the Angular HttpClient class to dump the incoming response to the browser console.

In the src\app\services folder, create a new file called:

```
debug.ts
```

Copy the following code into this file:

``` typescript
import { Observable } from 'rxjs';
import { tap } from 'rxjs/operators';

export enum RxJsLoggingLevel {
    TRACE,
    DEBUG,
    INFO,
    ERROR
}

let rxjsLoggingLevel = RxJsLoggingLevel.INFO;

export function setRxJsLoggingLevel(level: RxJsLoggingLevel) {
    rxjsLoggingLevel = level;
}

export const debug = (level: number, message: string) =>
    (source: Observable<any>) => source
        .pipe(
            tap(val => {
                if (level >= rxjsLoggingLevel) {
                    console.log(message + ': ', val);
                }
            })
        );
```

# Logging Verbosity Levels

When adding logging statements to your application, you must specify a LogLevel. The LogLevel allows you to control the verbosity of the logging output from your application.

## Log Levels

``` typescript
export enum RxJsLoggingLevel {
    TRACE,
    DEBUG,
    INFO,
    ERROR
}
```

Five levels of logging verbosity, ordered by increasing importance or severity:

**TRACE**
Used for the most detailed log messages, typically only valuable to a developer debugging an issue. These messages may contain sensitive application data and so should not be enabled in a production environment. Disabled by default. 

Example: 
```
Credentials: {"User":"someuser", "Password":"P@ssword"}
```

**DEBUG**
These messages have short-term usefulness during development. They contain information that may be useful for debugging but have no long-term value. This is the default most verbose level of logging. Disabled by default. 

Example: 
```
Entering method Configure with flag set to true
```

**INFO**
These messages are used to track the general flow of the application. These logs should have some long term value, as opposed to Verbose level messages, which do not. 

Example: 
```
Request received for path /foo
```

**ERROR**
An error should be logged when the current flow of the application must stop due to some failure, such as an exception that cannot be handled or recovered from. These messages should indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure. 

Example: 
```
Cannot insert record due to duplicate key violation
```

In the terminal window, verify there are no issues reported:

![image.png](/.attachments/image-a5381f82-8d39-4693-bca2-57286c742a8a.png)

Run the app.

```
npm run start
```

You have now added the Http Client Debug function.