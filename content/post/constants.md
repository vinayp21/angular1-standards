+++
draft = false
title = "Constants"

+++

<b>Constants</b>
A constant can be injected everywhere. A constant cannot be intercepted by a decorator, that means that the value of a constant should never be changed (though it is still possible to change it programmatically in Angular 1.x).

```javascript

angular.module('app', []);

app.constant('MOVIE_TITLE', 'The Matrix');

.controller('MyController', function (MOVIE_TITLE) {
  expect(MOVIE_TITLE).toEqual('The Matrix');
});


```

<b>Value</b>

A value is nothing more than a simple injectable value. The value can be a string, number but also a function. Value differs from constant in that value cannot be injected into configurations, but it can be intercepted by decorators.

```javascript

angular.module('app', []);

.value('movieTitle', 'The Matrix');

.controller('MyController', function (movieTitle) {
  expect(movieTitle).toEqual('The Matrix');
});

```
