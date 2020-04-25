You can use third party JavaScript files in your Angular SPA that are not necessarily Angular-ready.

# Install JavaScript Library

Install your 3rd party library, as follows: 

```
yarn add library-name
``` 

and import it in your code. 

If the library does not include typings, then it may be possible to separately install the typings using npm: 

```
yarn add @types/library_name --dev
```

As an example, if you wish to use some of the bootstrap controls that require additional JavaScript dependencies that are not written using the CommonJS standard, you can add references to them in the "scripts" section in the **angular.json file**, as follows: 

``` json
"scripts": [
    "node_modules/jquery/dist/jquery.slim.min.js",
    "node_modules/popper.js/dist/umd/popper.min.js",
    "node_modules/bootstrap/dist/js/bootstrap.min.js"
]
```

Not that for Bootstrap JavaScript, you could optimize even further by loading individual control scripts. 

Observe original files before adding the scripts to the **angular.json** file. 

![image.png](/.attachments/image-1d2d9611-24d6-44ab-804c-e1f6842d2d70.png)

Note that you must stop and restart the Angular SPA to implement any configuration changes you made to the angular.json file.

Observe the new addition of the scripts.js files after adding the scripts to the **angular.json** file.) 

![image.png](/.attachments/image-61562563-7b24-42ee-b2bb-8816aed61967.png)

