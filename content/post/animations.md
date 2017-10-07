+++
draft = false
title = "Animations"

+++

Use subtle animations with Angular to transition between states for views and primary visual elements. The 3 keys are subtle, smooth, seamless.

  1.	Subtle animations can improve User Experience when used appropriately.
  2.	Subtle animations can improve perceived performance as views transition.
  3.	Long animations can have the reverse effect on User Experience and perceived performance by giving the appearance of a slow application. Preferably  start with 300ms and adjust until appropriate.

<b>Getting Started</b>

To make your applications ready for animations, you must include the AngularJS Animate library:

	<script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.4.8/angular-animate.js"></script> 

Add ngAnimate as a dependency in your application module:	
	
	<body ng-app="myApp">
		<h1>Hide the DIV: <input type="checkbox" ng-model="myCheck"></h1>
		<div ng-hide="myCheck"></div>
	<script>
		var app = angular.module('myApp', ['ngAnimate']);
	</script>

<b>What does ngAnimate do ?</b>

	The ngAnimate module does not animate your HTML elements, but when ngAnimate notice certain events, like hide or show of an HTML element, the element gets some pre-defined classes which can be used to make animations.

	The directives in AngularJS who add/remove classes are:

	ng-show    
	ng-hide	   
	ng-class
	ng-view
	ng-include
	ng-repeat  
	ng-if
	ng-switch

	The ng-show and ng-hide directives adds or removes a ng-hide class value.

	The other directives adds a ng-enter class value when they enter the DOM, and a ng-leave attribute when they are removed from the DOM.

	The ng-repeat directive also adds a ng-move class value when the HTML element changes position.

	In addition, during the animation, the HTML element will have a set of class values, which will be removed when the animation has finished. Example: the ng-hide directive will add these class values:

	ng-animate
	ng-hide-animate
	ng-hide-add (if the element will be hidden)
	ng-hide-remove (if the element will be showed)
	ng-hide-add-active (if the element will be hidden)
	ng-hide-remove-active (if the element will be showed)

<b>Animations using CSS.</b>

CSS transitions

When the DIV element gets the .ng-hide class, the transition will take 0.5 seconds, and the height will smoothly change from 100px to 0:

	<style>
	div {
    	transition: all linear 0.5s;
    	background-color: lightblue;
    	height: 100px;
	}
	.ng-hide {
    	height: 0;
	}	
	</style>


CSS Animations allows you to change CSS property values smoothly, from one value to another, over a given duration:

	Example:
	When the DIV element gets the .ng-hide class, the myChange animation will run, which will smoothly change the height from 100px to 0:

	<style>
	@keyframes myChange {
    	from {
      	  height: 100px;
    	} to {
      	  height: 0;
    	}
	}
	div {
   	 height: 100px;
   	 background-color: lightblue;
	}
	div.ng-hide {
   	 animation: 0.5s myChange;
	}
	</style>