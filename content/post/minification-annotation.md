+++
draft = false
title = "Minification & Annotation practices"

+++
As discussed before, Minifying an Angular application is never easy, it’s a cumbersome process, if one dependency is found missing in a particular module’s controller / service the complete application will start breaking.

So your question would be is there a better in dealing with this problem? Of Course Yes. The best part is this can be automated too using Gulp or Grunt.

So your development involves task automations from Gulp / Grunt.

<b>ng-annotate</b>

Use ng-annotate for <b>Gulp</b> or <b>Grunt</b> and comment functions that need automated dependency injection using `/* @ngInject */`

This safeguards your code from any dependencies that may not be using minification-safe practices.

How does it work?

Write your controller in the below manner.

```javascript

angular
    .module('app')
    .controller('AvengersController', AvengersController);

/* @ngInject */
function AvengersController(storage, avengerService) {
    var vm = this;
    vm.heroSearch = '';
    vm.storeHero = storeHero;

    function storeHero() {
        var hero = avengerService.find(vm.heroSearch);
        storage.save(hero.name, hero);
    }
}


```

And build application via ng-annotate with your favourite task automation either gulp / grunt. It will produce the following output with $inject annotation and become minification safe code.

```javascript

angular
    .module('app')
    .controller('AvengersController', AvengersController);

/* @ngInject */
function AvengersController(storage, avengerService) {
    var vm = this;
    vm.heroSearch = '';
    vm.storeHero = storeHero;

    function storeHero() {
        var hero = avengerService.find(vm.heroSearch);
        storage.save(hero.name, hero);
    }
}

AvengersController.$inject = ['storage', 'avengerService'];


```

See as simple as that !!!.

<b>Note:</b> If ng-annotate detects injection has already been made (e.g. @ngInject was detected), it will not duplicate the $inject code.

<b>Note:</b> When using a route resolver you can prefix the resolver's function with `/* @ngInject */` and it will produce properly annotated code, keeping any injected dependencies minification safe.

```javascript

// Using @ngInject annotations
function config($routeProvider) {
    $routeProvider
        .when('/avengers', {
            templateUrl: 'avengers.html',
            controller: 'AvengersController',
            controllerAs: 'vm',
            resolve: { /* @ngInject */
                moviesPrepService: function(movieService) {
                    return movieService.getMovies();
                }
            }
        });
}

```

<b>Note:</b> Starting from Angular 1.3 you can use the ngApp directive's ngStrictDi parameter to detect any potentially missing minification safe dependencies. When present the injector will be created in "strict-di" mode causing the application to fail to invoke functions which do not use explicit function annotation (these may not be minification safe). Debugging info will be logged to the console to help track down the offending code. I prefer to only use ng-strict-di for debugging purposes only. <body ng-app="APP" ng-strict-di>.

Still wondering how the task automation works. Below I have given a Gulp automation task, which should clear your doubt.

```javascript

var gulp = require('gulp');
var ngAnnotate = require('gulp-ng-annotate');

gulp.task('default', function () {
    return gulp.src('src/app.js')
        .pipe(ngAnnotate())
        .pipe(gulp.dest('dist'));
});

```
We first install gulp and gulp-ng-annotate dependent modules from npm registry. Create a default gulp task, which simple says take the app.js file, feed it to ngAnnotate (i.e by pipe), ngAnnotate will look for ngInject syntax and injects the dependencies via $inject and copy the annotate file to dist folder.

As simple as that !!!.
