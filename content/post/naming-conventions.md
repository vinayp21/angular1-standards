+++
draft = false
title = "Naming Conventions"

+++

Using consistent names for all components while building Angular application, is utmost important.  

   1. Naming conventions help provide a consistent way to find content at a glance. Consistency within the project is vital. Consistency with a team is important. Consistency across a company provides tremendous efficiency.
   2. The naming conventions should simply help you find your code faster and make it easier to understand.

The Recommended way of naming files and structuring them is as below.

|-- app.js
|-- dashboard/
|   |-- dashboard.html
|   |-- dashboard.scss
|   |-- dashboard.directive.js
|   |-- dashboard.controller.js
|-- login/
|   |-- login.html
|   |-- login.scss
|   |-- login.directive.js
|   |-- login.controller.js
|-- inbox/
|   |-- inbox.html
|   |-- inbox.scss
|   |-- inbox.directive.js
|   |-- inbox.controller.js


```

// controllers
avengers.controller.js
avengers.controller.spec.js

// services/factories
logger.service.js
logger.service.spec.js

// constants
constants.js

// module definition
avengers.module.js

// routes
avengers.routes.js
avengers.routes.spec.js

// configuration
avengers.config.js

// directives
avenger-profile.directive.js
avenger-profile.directive.spec.js

```
<b>Naming Test files</b>

Name test specifications similar to the component they test with a suffix of spec, provides a consistent way to quickly identify components.

```

avengers.controller.spec.js
logger.service.spec.js
avengers.routes.spec.js
avenger-profile.directive.spec.js

```

<b>Controller Names</b>

Use consistent names for all controllers named after their feature. Use UpperCamelCase for controllers, as they are constructors.

```javascript

// avengers.controller.js
angular
    .module
    .controller('HeroAvengersController', HeroAvengersController);

function HeroAvengersController() { }

```

<b>Factory and Service Names</b>

Use consistent names for all -factories and services named after their feature. Use camel-casing for services and factories. Avoid prefixing factories and services with $. Only suffix service and factories with Service when it is not clear what they are (i.e. when they are nouns).

```javascript

// credit.service.js
angular
    .module
    .factory('creditService', creditService);

function creditService() { }

// customer.service.js
angular
    .module
    .service('customerService', customerService);

function customerService() { }

```

<b>Directive Component Names</b>

Use consistent names for all directives using camel-case. Use a short prefix to describe the area that the directives belong (some example are company prefix or project prefix).

```javascript
// avenger-profile.directive.js
angular
    .module
    .directive('xxAvengerProfile', xxAvengerProfile);

// usage is <xx-avenger-profile> </xx-avenger-profile>

function xxAvengerProfile() { }

```

<b>Modules</b>

When there are multiple modules, the main module file is named app.module.js while other dependent modules are named after what they represent. For example, an admin module is named admin.module.js. The respective registered module names would be app and admin.

<b>Configuration</b>

Separate configuration for a module into its own file named after the module. A configuration file for the main app module is named app.config.js (or simply config.js). A configuration for a module named admin.module.js is named admin.config.js.


<b>Routes</b>

Separate route configuration into its own file. Examples might be app.route.js for the main module and admin.route.js for the admin module. Even in smaller apps I prefer this separation from the rest of the configuration.
