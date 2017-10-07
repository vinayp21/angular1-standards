+++
draft = false
title = "Services, Factories & Providers"

+++

There are 3 ways to instantiate a service in angular JS

   * Service
   * Factory
   * Provider

Services

Syntax: module.service( 'serviceName', function ); 
Result: When declaring serviceName as an injectable argument you will be provided with an instance of the function. In other words new FunctionYouPassedToService().

Factories

Syntax: module.factory( 'factoryName', function ); 
Result: When declaring factoryName as an injectable argument you will be provided with the value that is returned by invoking the function reference passed to module.factory.

Providers

Syntax: module.provider( 'providerName', function ); 
Result: When declaring providerName as an injectable argument you will be provided with (new ProviderFunction()).$get(). The constructor function is instantiated before the $get method is called - ProviderFunction is the function reference passed to module.provider.

Providers have the advantage that they can be configured during the module configuration phase.

Here is the link from stackoverflow for a detailed explanation apart from the one given above

http://stackoverflow.com/questions/15666048/angularjs-service-vs-provider-vs-factory

Services in Angular JS are singletons, Services are instantiated with the new keyword, use this for public methods and variables.

<b>Example</b>

```javascript

angular
    .module('app')
    .service('logger', logger);

function logger() {
  this.logError = function(msg) {
    /* */
  };
}


```

Factories are singletons and return an object that contains the members of the service.

<b>Example</b>

```javascript
angular
    .module('app')
    .factory('logger', logger);

function logger() {
    return {
        logError: function(msg) {
          /* */
        }
   };
}
```

Choosing between Service and Factory : Which one should we use ?

It turns out that when we call service() it actually calls factory()and passes a function that asks the injector to instantiate an object by the given constructor.

It’s actually better to use services when it comes to migrating to ES6. 
Reason:  Service is a constructor function and a factory is not. Working with constructor functions in ES5 allows us to easily use ES6 classes when we migrate to ES6. (An ES6 class is really just a constructor function in ES5.)
With factories, this is not possible because they are simply called as functions.

Expose the callable members of the service (its interface) at the top, using a technique derived from the Revealing Module Pattern.

<b>Reason</b>

   * Placing the callable members at the top makes it easy to read and helps you instantly identify which members of the service can be called and must be unit tested (and/or mocked).
   * This is especially helpful when the file gets longer as it helps avoid the need to scroll to see what is exposed.
   * Setting functions as you go can be easy, but when those functions are more than 1 line of code they can reduce the readability and cause more scrolling. Defining the callable interface via the returned service moves the implementation details down, keeps the callable interface up top, and makes it easier to read.

```javascript

/* avoid */
function dataService() {
  var someValue = '';
  function save() {
    /* */
  };
  function validate() {
    /* */
  };

  return {
      save: save,
      someValue: someValue,
      validate: validate
  };
}
/* recommended */
function dataService() {
    var someValue = '';
    var service = {
        save: save,
        validate: validate
    };
    return service;

    ////////////

    function save() {
        /* */
    };

    function validate() {
        /* */
    };
}


```

<b>Separate Data Calls</b>

Refactor logic for making data operations and interacting with data to a factory. Make data services responsible for XHR calls, local storage, stashing in memory, or any other data operations.

<b>Reason</b>

The controller's responsibility is for the presentation and gathering of information for the view. It should not care how it gets the data, just that it knows who to ask for it. Separating the data services moves the logic on how to get it to the data service, and lets the controller be simpler and more focused on the view.

<b>Example</b>

```javascript

angular
    .module('app.core')
    .factory('dataservice', dataservice);

dataservice.$inject = ['$http', 'logger'];

function dataservice($http, logger) {
    return {
        getAvengers: getAvengers
    };

    function getAvengers() {
        return $http.get('/api/maa')
            .then(getAvengersComplete)
            .catch(getAvengersFailed);

        function getAvengersComplete(response) {
            return response.data.results;
        }

        function getAvengersFailed(error) {
            logger.error('XHR Failed for getAvengers.' + error.data);
        }
    }
}


```
