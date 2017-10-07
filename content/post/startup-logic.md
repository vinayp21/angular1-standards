+++
draft = false
title = "Startup Logic"

+++

As we all know, Angular application gets bootstrapped with config block and run block before rendering the view and binding the variables from module to view.

Which is an ideal candidate for writing all of the Startup Logic of your Angular application in these two block.

<b>Configuration</b>

Inject code into module configuration that must be configured before running the angular app. Ideal candidates include providers and constants.

This makes it easier to have less space for configurations.

```javascript

angular
    .module('app', [‘ngRoute’])
    .config(config);

configure.$inject =
    ['$routeProvider'];

function config ($routeProvider) {
    $routeProvider
    .when("/banana", {
        template : "<h1>Banana</h1><p>Bananas contain around 75% water.</p>"
    })
    .when("/tomato", {
        template : "<h1>Tomato</h1><p>Tomatoes contain around 95% water.</p>"
    })
    .otherwise({
        template : "<h1>None</h1><p>Nothing has been selected,</p>"
    });
}

```

<b>Run block.</b>

Any code that needs to run when the application starts should be declared in a factory, i.e return a function and injected into the run block.

Code return directly in a run block can be difficult to test, because this needs to the complete application to be tested along with it. Placing them in factories is much easier to test them.

```javascript

angular
    .module('app')
    .run(runBlock);

runBlock.$inject = ['authenticator'];

function runBlock(authenticator) {
    authenticator.initialize();
}

angular.module(‘app’)
	.factory(‘authenticator’, authenticator);

function authenticator() {
var factory = this;

factory.initialize = function() {
	//initialize code.
}

return factory;
}

```
