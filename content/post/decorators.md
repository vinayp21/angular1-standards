+++
draft = false
title = "Decorators"

+++

Decorators are a design pattern that is used to separate modification or decoration of a class without modifying the original source code. In Angular, decorators are functions that allow a service, directive or filter to be modified prior to its usage.

So in other words, decorator can intercept service instance created by factory, service, value, provider, and gives the options to change some instance (service) that is otherwise not configurable / with options.

Some practical uses of decorators:

	* Caching: if we have a service which makes potentially expensive HTTP calls, we can wrap the service in a caching decorator which checks local storage before making the external call.

	* Debugging/Tracing: have a switch depending on your development/production configuration which decorates your services with debugging or tracing wrappers.

	* Throttling : wrap frequently triggered calls in a debouncing wrapper. Allows us to easily interact with rate-limited services, for example.


There are two ways to register decorators.

  * $provide.decorator (In Angular 1.4.x modules have decorator method, this method no longer needed)
  * module.decorator

Each provide access to a $delegate, which is the instantiated service/directive/filter, prior to being passed to the service that required it.

	* A good use case of $provide.decorator is when you need to do minor "tweak" on some third-party/upstream service, on which your module depends, while leaving the service intact (because you are not the owner/maintainer of the service). 

<b>Example and Reference</b>

[Angular Decorators](https://docs.angularjs.org/guide/decorators)
