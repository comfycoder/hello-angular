Open up a command line window and run the following command:

```
ng new my-app --routing --style=scss --skipInstall=true
```

You should see some output like the following:

![image.png](/.attachments/image-8dcdc00d-56bf-4909-a672-db01ea492c0c.png)

You may also see some warning statements in the terminal output like the following, but they are of no consequence and you can ignore them.

![image.png](/.attachments/image-3fdaa118-dc9a-4b0c-8cf7-37c948f9a1f6.png)

Navigate into the **my-app** folder and run the following in the command line window.

```
code .
```

![image.png](/.attachments/image-8a296813-3d55-48d6-8e7f-ae7ea5799d8d.png)

Visual Studio Code launches showing the content of the my-app project folder.

![image.png](/.attachments/image-1cf23277-d4d6-458b-9632-f934e66a2d8f.png)

You have successfully created a new Angular app in a folder call **my-app** and opened it in Visual Studio Code.

It supports routing to components (pages) and uses SCSS for styling the application components.

The node modules have not yet been downloaded.

From the VS Code Terminal menu, select the New Terminal menu item.

![image.png](/.attachments/image-c10f3b2b-aa6e-4e75-b466-ffb87ae158be.png)

VS Code opens up a new Terminal window.

![image.png](/.attachments/image-07dd53e1-f787-4911-a63c-8c365f6c6ec6.png)

In the terminal window, execute the following on the command line:

```
yarn install
```

![image.png](/.attachments/image-2db25040-fabe-4a22-8c82-645091117a1c.png)

The Yarn CLI tool reads the package.json file, creates a node_modules folder, and downloads the specified npm packages.

![image.png](/.attachments/image-8fbd1c3b-ac85-4fb0-83e8-cc2a7fc28336.png)

In the terminal window, execute the following on the command line:

```
npm run start
```

![image.png](/.attachments/image-1d75a9b7-7885-4ca1-9ce9-1ed6819eb3c0.png)

This tells the **npm** tool to launch the Angular app.

You may see the following the first time you run the app:

![image.png](/.attachments/image-a2764679-1784-4725-a297-4d9126612b53.png)

Type **N** and press the **Enter** key to continue.

Note that initially, **npm** compiles several Angular libraries and then compiles and starts the app using its built-in web server.

![image.png](/.attachments/image-7845f3c8-d3a5-4c79-aa31-3c3120cfc158.png)

Note that the default script configuration does not launch the app in your browser.

As indicated in the terminal output, open a browser and navigate the following URL:

```
http://localhost:4200
```

The Angular app should look something like the following:

![image.png](/.attachments/image-73e724ce-a4f6-4ae4-a008-713d3e89bda4.png)

You have successfully created a new Angular SPA!