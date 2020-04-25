[Home](README.md)

# Configure And Explore NPM Scripts

## Upgrade The Scripts Block 

Edit the **package.json** file. 

Replace the scripts block with the following: 

``` json
"scripts": {
    "ngver": "ng version",
    "start": "ng serve --open",
    "start:prod": "ng serve --prod --open",
    "build": "ng build",
    "build:prod": "ng build --prod",
    "build:prod:folder": "ng build --prod --base-href=./",
    "test": "ng test",
    "lint": "ng lint --type-check --format stylish",
    "lint:fix": "ng lint --type-check --fix --format stylish",
    "lint:ci": "ng lint --type-check",
    "e2e": "ng e2e"
}
```

## Check The Current Angular Version 

From the terminal window, run the **ngver** script. 

```
npm run ngver
```

You should see output similar to the following:

![image.png](/.attachments/image-9cb7ec1c-ee36-4f8b-8a12-e478b1a70076.png)

## Do A Development Build 

From the terminal window, run the **build** script. 

```
npm run build
```

You should see output similar to the following:

![image.png](/.attachments/image-735e8024-f5b1-4541-ad87-4a2be32107af.png)

In the files view in VS Code, expand the new dist folder that was created.

![image.png](/.attachments/image-9409243c-ee70-4479-9eed-6e9fc1de58ac.png)

The new transpiled (think compiled JavaScript) files were created in the project's **dist/my-app** folder.

Examine the contents of this folder in Windows Explorer.

![image.png](/.attachments/image-d6eb5d07-bd3e-4c62-bc34-ec6ed14ac115.png)

Make a mental note of the files present and the relative sizes of the files. The development build contains *.map files to support debugging code in a browser, and the file sizes are larger than you would see in a production build.

## Do A Production Build 

From the terminal window, run the **build:prod** script. 
 
```
npm run build:prod
```

You should see output similar to the following:

![image.png](/.attachments/image-7e025103-8ebc-4021-97d9-e7282d72bebb.png)

The new transpiled (think compiled JavaScript) files were created in the project's **dist/my-app** folder.

Examine the contents of this folder in Windows Explorer.

![image.png](/.attachments/image-efe8566d-afe5-4af9-a7c3-b6462f224171.png)

Notice that there are fewer files (no *.map files) were produced. The files are much smaller than the ones produced for the development build. The file names are also modified with a hash value that changes whenever you modify code. This ensures that the latest files are sent to the browser.

The ng build command with the --prod meta-flag (ng build --prod) compiles with AOT by default. For more information see [Angular - Ahead-of-time (AOT) compilation](https://angular.io/guide/aot-compiler).

## Run The Start Task  

From the terminal window, run the **start** script.
 
```
npm run start
```

You should see output similar to the following:

![image.png](/.attachments/image-454cf2a5-cae4-483a-8e65-3b15289ed897.png)

The angular lightweight web server automatically launches the application in your default browser using a development build.

![image.png](/.attachments/image-72cfb727-6c29-40aa-ae45-7a657a034bd7.png)

[Home](README.md)

[Add The Bootstrap Framework](Add-The-Bootstrap-Framework.md)