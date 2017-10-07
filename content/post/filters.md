+++
draft = false
title = "Filters"

+++

Filters format the value of an expression for display to the user. They can be used in view templates, controllers or services. Angular comes with a collection of built-in filters, but it is easy to define your own as well.

The underlying API is the `$filterProvider`.

In templates, filters are only executed when their inputs have changed. This is more performant than executing a filter on each $digest as is the case with expressions.

There are two exceptions to this rule:

   * In general, this applies only to filters that take primitive values as inputs. Filters that receive Objects as input are executed on each$digest, as it would be too costly to track if the inputs have changed.
   * Filters that are marked as $stateful are also executed on each $digest. See Stateful filters for more information. Note that no Angular core filters are $stateful.

<b>Example and Reference</b>

[Angular Filter Guide](https://docs.angularjs.org/guide/filter)

[Filters in Angular](http://www.w3schools.com/angular/angular_filters.asp)
