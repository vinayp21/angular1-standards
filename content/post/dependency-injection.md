+++
draft = false
title = "Manual Annotating for Dependency Injection"

+++
Dependency Injection can be very infectious if not handled properly. Especially when minifying the code for production, if the dependencies are not specified in the appropriate order, the behavior of the application is not as expected.

Take this code for example.

```javascript

angular
    .module('app')
    .controller('MyController',function($scope) {
	$scope.doSomething = function() {
		// some code here
	}
});

```

This is how we were taught to write a controller in the beginning, but when minified this sort breaks.

Here is the same above code after it is minified.

```javascript

angular.module("app").controller("MyController",function(a){a.doSomething=function(){}});
```

Clearly angular compiler does not know what a, it can recognize $scope, but a?? Hmm..

Let’s look at the recommended ways of writing them.

   <b>Manually identifying Dependencies.</b>


   ```javascript

       angular
        .module('app')
        .controller('MyController', [‘$scope’, function($scope) {
    	$scope.doSomething = function() {
    		// some code here
    	}
    }]);

   ```

   This is one way of writing controller, we are specifying the dependencies within an array and function maps the each of its dependencies according to their order.

   This is the look fairly enough, but certainly not the best way. This still fails, for example

   1. Having a number of dependencies which will be very hard to read.
   2. And more importantly the order in which they are specified, if the order changes the whole app is not broken.

   So, the better way of writing it is user $inject.

   $inject is used to manually identify your dependencies for Angular components.

   Why we use it?

   1.	This safeguards your dependencies from being vulnerable to minification issue when parameters maybe mangled.
   2.	Avoids creating in-line dependencies as long lists can be difficult to read in the array, also can be confusing as the array is a series of strings while the last item is the component’s function.


```javascript

/* recommended */
angular
    .module('app')
    .controller('DashboardController', DashboardController);

    DashboardController.$inject = ['$location', '$routeParams', 'common', 'dataservice'];

   function DashboardController($location, $routeParams, common, dataservice) {

   }

```
<b>Note:</b> When your function is below a return statement the $inject may be unreachable (this may happen in a directive).

```javascript

/* avoid */
// inside a directive definition
function outer() {
    var ddo = {
        controller: DashboardPanelController,
        controllerAs: 'vm'
    };
    return ddo;

    DashboardPanelController.$inject = ['logger']; // Unreachable
    function DashboardPanelController(logger) {
    }
}

```

```javascript

/* recommended */
// outside a directive definition
function outer() {
    var ddo = {
        controller: DashboardPanelController,
        controllerAs: 'vm'
    };
    return ddo;
}

DashboardPanelController.$inject = ['logger'];
function DashboardPanelController(logger) {
}

```

<b>Manually Identify Route Resolver Dependencies.</b>

$inject can be used to manually identify your route resolver dependencies for Angular components.
This technique breaks out the anonymous function for the route resolver, making it easier to read.

```javascript

angular.module(‘app’)
	.config(config);

function config($routeProvider) {
	$routeProvider
		.when(‘/’, {
			templateUrl: ‘avengers.html’,
			controller: ‘AvengersController’,
			controllerAs, vm,
			resolve: {
				moviesPrepService: moviesPrepService
			}
		}
};

moviesPrepService.$inject = ['movieService'];
function moviesPrepService(movieService) {
    return movieService.getMovies();
}

```
