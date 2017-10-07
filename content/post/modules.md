+++
draft = false
title = "Modules"

+++

A module is the main way to define an AngularJS app. The module of an app is where
we’ll contain all of our application code. An app can contain several modules, each one containing code that pertains to specific functionality.

### Naming Conventions
Use unique naming conventions with separators for sub-modules.

<b>Reason</b> - Unique names help avoid module name collisions. Separators help define modules and their sub module hierarchy. For example app may be your root module while `app.dashboard` and `app.users` may be modules that are used as dependencies of app.

## Declaring the Module
It is recommended to declare modules without a variable using the setter syntax.

Reason :  With 1 component per file, there is rarely a need to introduce a variable for the module

<b>Example</b>

```javascript
  /* avoid */
  var app = angular.module('app', [
      'ngAnimate',
      'ngRoute',
      'app.shared',
      'app.dashboard'
  ]);
```

```javascript

/* recommended */
angular
    .module('app', [
        'ngAnimate',
        'ngRoute',
        'app.shared',
        'app.dashboard'
    ]);

```

## Getters
When using a module, it is better to avoid using a variable and instead use chaining with the getter syntax.
<b>Example</b>

Reason : This produces more readable code and avoids variable collisons or leaks.

```javascript
  angular
      .module('app')
      .controller('SomeController', SomeController);

  function SomeController() { }

```

## Avoid Anonymous functions

Use named functions instead of passing an anonymous function in as a callback.

This produces more readable code, is much easier to debug, and reduces the amount of nested callback code.

Reason : This produces more readable code, is much easier to debug, and reduces the amount of nested callback code. 

```javascript
/* avoid */
angular
    .module('app')
    .controller('DashboardController', function() { })
    .factory('logger', function() { });

```

```javascript
/* recommended */
angular
    .module('app')
    .controller('DashboardController', DashboardController);

function DashboardController() { }

```
