+++
draft = false
title = "Providers"

+++

The Provider recipe is syntactically defined as a custom type that implements a $get method. This method is a factory function just like the one we use in the Factory recipe. In fact, if you define a Factory recipe, an empty Provider type with the $get method set to your factory function is automatically created under the hood.

You should use the Provider recipe only when you want to expose an API for application-wide configuration that must be made before the application starts. This is usually interesting only for reusable services whose behavior might need to vary slightly between applications.

<b>Example and Reference</b>

[The Providers Recipe](http://www.learn-angular.org/#!/lessons/the-provider-recipe)
