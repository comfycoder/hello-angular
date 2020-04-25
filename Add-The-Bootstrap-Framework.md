# Install Bootstrap

From the terminal window, run the following Yarn commands:

```
yarn add bootstrap
yarn add jquery@3.4.1
yarn add popper.js
yarn add --dev @types/jquery
```

Note: jQuery 3.4.1 is specified due to a very recent breaking change.

You should see output similar to the following:

![image.png](/.attachments/image-eef42f4d-9d94-4a8d-9620-da43d465cb08.png)

Open the package.json file, and your should see the NPM packages you just added:

![image.png](/.attachments/image-21d472bd-cef3-4bc4-bf02-64ff328c282f.png)

These files were also added to the Y

# Add Local Bootstrap Override Files 

In the VS Code files area, we'll create some new files.

In the **src** folder, create a new file called: 

```
_variables.scss
```

Add nothing to this file at this time. 

In the **src** folder, create a new file called: 

```
_bootstrap.scss
```

![image.png](/.attachments/image-ab075246-1dea-4036-913d-ce405d0b022d.png)

Add the following content to the **_bootstrap.scss** by copying it from the **bootstrap.scss** source file in the **node_module/bootstrap/scss** folder. 

``` css
/*!
 * Bootstrap v4.4.1 (https://getbootstrap.com/)
 * Copyright 2011-2019 The Bootstrap Authors
 * Copyright 2011-2019 Twitter, Inc.
 * Licensed under MIT (https://github.com/twbs/bootstrap/blob/master/LICENSE)
 */

@import "functions";
@import "variables";
@import "mixins";
@import "root";
@import "reboot";
@import "type";
@import "images";
@import "code";
@import "grid";
@import "tables";
@import "forms";
@import "buttons";
@import "transitions";
@import "dropdown";
@import "button-group";
@import "input-group";
@import "custom-forms";
@import "nav";
@import "navbar";
@import "card";
@import "breadcrumb";
@import "pagination";
@import "badge";
@import "jumbotron";
@import "alert";
@import "progress";
@import "media";
@import "list-group";
@import "close";
@import "toasts";
@import "modal";
@import "tooltip";
@import "popover";
@import "carousel";
@import "spinners";
@import "utilities";
@import "print";
```

Then alter the import statements to include the paths to each of the individual SCSS source files.

``` scss
/*!
 * Bootstrap v4.4.1 (https://getbootstrap.com/)
 * Copyright 2011-2019 The Bootstrap Authors
 * Copyright 2011-2019 Twitter, Inc.
 * Licensed under MIT (https://github.com/twbs/bootstrap/blob/master/LICENSE)
 */

@import "../node_modules/bootstrap/scss/functions";
@import "../node_modules/bootstrap/scss/variables";
@import "../node_modules/bootstrap/scss/mixins";
@import "../node_modules/bootstrap/scss/root";
@import "../node_modules/bootstrap/scss/reboot";
@import "../node_modules/bootstrap/scss/type";
@import "../node_modules/bootstrap/scss/images";
@import "../node_modules/bootstrap/scss/code";
@import "../node_modules/bootstrap/scss/grid";
@import "../node_modules/bootstrap/scss/tables";
@import "../node_modules/bootstrap/scss/forms";
@import "../node_modules/bootstrap/scss/buttons";
@import "../node_modules/bootstrap/scss/transitions";
@import "../node_modules/bootstrap/scss/dropdown";
@import "../node_modules/bootstrap/scss/button-group";
@import "../node_modules/bootstrap/scss/input-group";
@import "../node_modules/bootstrap/scss/custom-forms";
@import "../node_modules/bootstrap/scss/nav";
@import "../node_modules/bootstrap/scss/navbar";
@import "../node_modules/bootstrap/scss/card";
@import "../node_modules/bootstrap/scss/breadcrumb";
@import "../node_modules/bootstrap/scss/pagination";
@import "../node_modules/bootstrap/scss/badge";
@import "../node_modules/bootstrap/scss/jumbotron";
@import "../node_modules/bootstrap/scss/alert";
@import "../node_modules/bootstrap/scss/progress";
@import "../node_modules/bootstrap/scss/media";
@import "../node_modules/bootstrap/scss/list-group";
@import "../node_modules/bootstrap/scss/close";
@import "../node_modules/bootstrap/scss/toasts";
@import "../node_modules/bootstrap/scss/modal";
@import "../node_modules/bootstrap/scss/tooltip";
@import "../node_modules/bootstrap/scss/popover";
@import "../node_modules/bootstrap/scss/carousel";
@import "../node_modules/bootstrap/scss/spinners";
@import "../node_modules/bootstrap/scss/utilities";
@import "../node_modules/bootstrap/scss/print";
```

In the src folder, added the following statement to the top of the **styles.scss** file: 
 
``` scss
@import 'variables'; 
@import 'bootstrap';
```

![image.png](/.attachments/image-fc3c7252-a2ae-459a-b768-38942ca8550b.png)

These statements include the **_variables.scss** and **_bootstrap.scss** files you created in the **src** folder. 

# Test The Integration Of Bootstrap

Open a command line windows and run the start script. 

```
npm run start
``` 

The Angular SPA should appear something like the following:

![image.png](/.attachments/image-fbd80665-5c34-4339-87b0-75afff65c435.png)

Bootstrap is now overriding some of the starter app styles.

Before adding Boostrap, the **styles.js** development file size was 12.8KB, as shown here:

![image.png](/.attachments/image-c5b32d3d-feed-467e-8bcb-cf638deddc67.png)

After adding Boostrap, the **styles.js** development file size is now 600KB, as shown here:

![image.png](/.attachments/image-51e81096-f77e-4e2e-a0b2-ae81f777afd3.png)

You should run with all of the bootstrap styles until you near the end of your development cycle. 

# Optimize Your Styles CSS File Size 

Once you near the end of your development, you can optimize the Bootstrap stylesheet by commenting out unused styles in the **_bootstrap.scss** file. 

For example, you could comment out the following styles in the _bootstrap.scss file: 

``` scss
/*!
 * Bootstrap v4.4.1 (https://getbootstrap.com/)
 * Copyright 2011-2019 The Bootstrap Authors
 * Copyright 2011-2019 Twitter, Inc.
 * Licensed under MIT (https://github.com/twbs/bootstrap/blob/master/LICENSE)
 */

@import "../node_modules/bootstrap/scss/functions";
@import "../node_modules/bootstrap/scss/variables";
@import "../node_modules/bootstrap/scss/mixins";
@import "../node_modules/bootstrap/scss/root";
@import "../node_modules/bootstrap/scss/reboot";
@import "../node_modules/bootstrap/scss/type";
@import "../node_modules/bootstrap/scss/images";
// @import "../node_modules/bootstrap/scss/code";
@import "../node_modules/bootstrap/scss/grid";
@import "../node_modules/bootstrap/scss/tables";
@import "../node_modules/bootstrap/scss/forms";
@import "../node_modules/bootstrap/scss/buttons";
@import "../node_modules/bootstrap/scss/transitions";
@import "../node_modules/bootstrap/scss/dropdown";
@import "../node_modules/bootstrap/scss/button-group";
@import "../node_modules/bootstrap/scss/input-group";
@import "../node_modules/bootstrap/scss/custom-forms";
@import "../node_modules/bootstrap/scss/nav";
@import "../node_modules/bootstrap/scss/navbar";
@import "../node_modules/bootstrap/scss/card";
// @import "../node_modules/bootstrap/scss/breadcrumb";
// @import "../node_modules/bootstrap/scss/pagination";
// @import "../node_modules/bootstrap/scss/badge";
// @import "../node_modules/bootstrap/scss/jumbotron";
// @import "../node_modules/bootstrap/scss/alert";
// @import "../node_modules/bootstrap/scss/progress";
@import "../node_modules/bootstrap/scss/media";
@import "../node_modules/bootstrap/scss/list-group";
@import "../node_modules/bootstrap/scss/close";
// @import "../node_modules/bootstrap/scss/toasts";
@import "../node_modules/bootstrap/scss/modal";
@import "../node_modules/bootstrap/scss/tooltip";
@import "../node_modules/bootstrap/scss/popover";
// @import "../node_modules/bootstrap/scss/carousel";
@import "../node_modules/bootstrap/scss/spinners";
@import "../node_modules/bootstrap/scss/utilities";
@import "../node_modules/bootstrap/scss/print";
```

There are other ways to add Bootstrap to your project, but this approach gives you more control over which Bootstrap styles you wish to include in the final Angular SPA production build.






