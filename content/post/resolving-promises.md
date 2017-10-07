+++
draft = false
title = "Promises"

+++

Promises in general with Javascript is used for asynchronous computations, the same can be used in the Angular for asynchronous computations. When developing an Angular application, we can follow the below standards to avoid asynchronous behaviors.

<b>Controller Activation Promises</b>

* The usage of Controller Activation Promises solves the problem of having start-up logic scattered across the controller. Placing the start-up logic in a consistent place in controller makes it easier to locate the code, test the individual controller and helps avoid spreading of the controller start-up logic. 
* The controller activate makes it convenient to re-use the logic for a refresh of the controller/view, keeps the logic together, gets render UI view faster, makes animations easy with ng-view (ngRoute) or ui-view (ui-router).

When writing a controller function for a module we need to avoid the below coding practice.

```javascript

function AvengersController(dataservice) {
    var vm = this;
    vm.avengers = [];
    vm.title = 'Avengers';

    dataservice.getAvengers().then(function(data) {
        vm.avengers = data;
        return vm.avengers;
    });
}

```

Below is the recommended and standard way of writing a controller in angular app to avoid scattered start-up logic.

```javascript

function AvengersController(dataservice) {
    var vm = this;
    vm.avengers = [];
    vm.title = 'Avengers';

    activate();

    function activate() {
        return dataservice.getAvengers().then(function(data) {
            vm.avengers = data;
            return vm.avengers;
        });
    }
}

```

The advantages of writing a controller as above.

   1. The code executes only after the route and the controller’s activate function.
   2. The view starts to load right away.
   3. Data bindings kicks in when the activate promise resolves.


<b>Route Resolve Promises</b>

Route Resolve Promises are used with Angular routing frameworks like ngRoute or Angular UI Router. Uses of Route Resolve can be opinionated based on two scenarios.

   1. When the controller depends on a promise to be resolved before the controller is activated / start-up logic starts executing. Eg. When the controller depends on data to decide the next course of action / computation. Theses dependencies are resolved in $routeProvider in ngRoute or $stateProvider in Angular UI router before the controller logic is executed.
   
   2. Route resolve can also be used, when you want to decide to cancel the route before transitioning to the view. Eg. When the user needs to authenticated before transitioning to the view.

<b>Note:</b> The Route resolve code executes before the route via promises. Rejecting the promise cancels the route. Resolve makes the new view to wait for the route to resolve.

The standard approach for coding route resolve.

```javascript

angular
    .module('app')
    .config(config);

function config($routeProvider) {
    $routeProvider
        .when('/home', {
            templateUrl: 'home.html',
            controller: 'HomeController',
            controllerAs: 'vm',
            resolve: {
                itemService: function(itemService) {
                    return itemService.getItems();
                }
            }
        });
}

// avengers.js
angular
    .module('app')
    .controller('HomeController', HomeController);

HomeController.$inject = ['itemService'];
function HomeController(itemService) {
    var vm = this;
    vm.items = itemService.items;
}


```


<b>Handling Exceptions in Promises.</b>

Once a promise is rejected, due to an error, should always be handled via a catch block to maintain in the same promise chain.

Whenever writing services / factories, one should always handle exceptions or the user will not know what happened or what the error was. To avoiding swallowing errors and misinforming the user.

If the catch block does not return a rejected promise, the caller block of the promise function will not know what happened / exception that had occurred.

The recommended way of handling an exception is shown below.

 ```javascript

 angular
  .module('app')
  .service(‘CustomerService’, CustomerService);

  function CustomerService($http, $q) {
  	var service = this;

  	service.getCustomerDetails = function() {
  	   return $http({method: ‘GET’, url: ‘/api/getCustomers/’})
  		.then(successResponse)
  		.catch(errorResponse);
  	}

  	function successResponse(data, status, headers, config) {
  	   return data.data;
  	}

  	function errorResponse(e) {
  	  var newMessage = 'XHR Failed for getCustomer'
            if (e.data && e.data.description) {
             newMessage = newMessage + '\n' + e.data.description;
            }
            e.data.description = newMessage;
            logger.error(newMessage);
            return $q.reject(e);
  	}
  }

 ```
