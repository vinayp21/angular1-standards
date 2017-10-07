+++
draft = false
title = "Components (Building web components)"

+++

Web Components are a set of features that allow the creation of reusable widgets in web documents and web applications.

In Angular( >1.5x ), a Component is a special kind of directive that uses a simpler configuration which is suitable for a component-based application structure.

This makes it easier to write an app in a way that's similar to using Web Components or using Angular 2's style of application architecture.

Advantages of Components

   * Simpler configuration than plain directives.
   * Promote sane defaults and best practices.
   * Optimized for component-based architecture.
   * Writing component directives will make it easier to upgrade to Angular 2.

When not to use Components

   * For directives that need to perform actions in compile and pre-link functions, because they aren't available.
   * When you need advanced directive definition options like priority, terminal, multi-element.
   * When you want a directive that is triggered by an attribute or CSS class, rather than an element.

<b>Example and Reference</b>

[Angular 1.5 components](https://scotch.io/tutorials/how-to-use-angular-1-5s-component-method)

[Angular Component guide](https://docs.angularjs.org/guide/component)

[Building Web components in Angular](http://codepen.io/thomasnyambati/pen/gMaReM?editors=1010)
