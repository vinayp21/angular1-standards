+++
draft = false
title = "Angular $ Wrapper Services"

+++

When building an Angular applications avoid using default Javascript web API’s like document, window, setTimeout and setInterval, because Angular has a set of services provided that support these API’s.

   	<b>Use $document & $window instead of document and window of default Javascript API’s.</b>

   	These services are wrapped by Angular and more easily testable than using document and window in tests. This helps you avoid having to mock document and window yourself.

   	<b>Use $timeout and $interval instead of setTimeout and setInterval.</b>

   	These services are wrapped by Angular and more easily testable and handle Angular's digest cycle thus keeping data binding in sync.

	<b> Use $http instead of $.ajax.<b>

	$http is testable, sets the content type to 'application/json' for you on POST requests. returns a "promise" similar to other areas in angular, which means .success, .done are consistent with angular.

	<b>Use promises ($q) instead of callbacks.<b>

	A service that helps you run functions asynchronously, and use their return values (or exceptions) when they are done processing.
	Promises have uniform handling of return values and uncaught exceptions. With callbacks, how an exception gets handled may depend entirely on which of the many nested callbacks threw it, and which of the functions taking callbacks has a try/catch in its implementation.

	<b>Use $location instead of window.location.<b>

	Allow read/write access to the current browser location and knows about all internal life-cycle phases, integrates with $watch, .... seamless integration with HTML5 API.

	<b>$templateCache<b>

	The first time a template is used, it is loaded in the template cache for quick retrieval. You can load templates directly into the cache in a script tag, or by consuming the $templateCache service directly.

	angular.module('myApp', []);
		myApp.run(function($templateCache) {
  		$templateCache.put('templateId.html', 'This is the content of the template');
	});

	To retrieve the template later, simply use it in your component:
		myApp.component('myComponent', {
   		templateUrl: 'templateId.html'
		});

	or get it via the $templateCache service:
	$templateCache.get('templateId.html')

	Angular first looks into $templateCache, and if required file doesn't exist, it will make a request for it and will put it in the cache. But if it does exists, it is taken from there. So if you are using your directive more number of times per page, it will only make one request for your directive file, or if you navigate back and forth your app, it won't request files that were already loaded.

	$templateCache is simply a key-value store where you can get and put templates. Angular will automatically check $templateCache whenever you use templateUrl in your application or when you specify a template src with ng-includebefore attempting to retrieve your template from a remote location.
	Does it make sense to add $templateCache.removeAll(); on each controller? 
	Yes, only if it's really small application,

	For session-level cache you can use $cacheFactory. This should be used to cache results from requests or heavy computations.
