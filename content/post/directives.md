+++
draft = false
title = "Directives"

+++

Directives are one of the most powerful components of AngularJS, helping you extend basic HTML elements/attributes and create reusable and testable code.

<b>Types</b>

  Types         |     Usage     
  ---           |     ---
  Attribute     |     `<div book></div>`
  Class         |     `<div class="book"></div>`
  Element       |     `<book data="book_data"></book>`
  Comment       |     `<!--directive:book -->`


<b>Best Practices</b>

   * Create one directive per file. One directive per file is easy to maintain.
   * When manipulating the DOM directly, use a directive.
   * Use bindToController = true when using controller as syntax with a directive when you want to bind the outer scope to the directive's controller's scope.


<b>Using Attribute vs. Element</b>

   * Use your directive as an element name instead of attribute If the need is to create a component with core behavior and possibility to decorate the component with additional behavior in future.
   * Use your directive as an attribute instead of element name when you are adding functionality to an existing element.
   * Always it is recommended to restrict to element and attributes directive.


<b>Naming Convention</b>
Prefer using two or three letter prefix (except ng) while naming directives to avoid collision with future HTML releases. Using “ng” as prefix might collide with AngularJS OOTB directives in future.

```html
<!-- Avoid -->
<example>

<!-- Recommended-->
<my-directive>, <my-test-directive>
```
<b>Directive Scope</b>

Directives are one of the most powerful features of AngularJS. You can imagine them as building blocks ( aka re-usable components ) of any    AngularJS application.

All directives have a scope associated with them. They use this scope for accessing data/methods inside the template and link function. By default, unless explicitly set, directives don’t create their own scope. Therefore, directives use their parent scope ( usually a controller ) as their own.

However, AngularJS allows us to change the default scope of directives by passing a configuration object known as directive definition object.

<b>Different types of directive scopes-</b>
  Scope: False - Directive uses its parent scope 

  Example -

<div ng-app="test">
    
    <div ng-controller="Ctrl1">
        <h2 ng-click="reverseName()">Hey {{name}}, Click me to reverse your name</h2>
        <div my-directive class='directive'></div>
    </div>
</div>
```javascript
var app = angular.module("test",[]);

app.controller("Ctrl1",function($scope){
    $scope.name = "Harry";
    $scope.reverseName = function(){
        $scope.name = $scope.name.split('').reverse().join('');
    };
});

app.directive("myDirective", function(){
    return {
        restrict: "EA",
        scope: false,
        template: "<div>Your name is : {{name}}</div>"+
    };
});
```

  Since there’s no scope provided in the DDO, the directive uses its parent scope

<b>Scope: True - Directive gets a new scope-</b>
This is achieved by setting a “true” value to the scope property of the DDO. When directive scope is set to “true”, AngularJS will create a new scope object and assign to the directive. This newly created scope object is prototypically inherited from its parent scope ( the controller scope where it’s been used ).

When scope is set to “true”, AngularJS will create a new scope by inheriting parent scope ( usually controller scope, otherwise the application’s rootScope ). Any changes made to this new scope will not reflect back to the parent scope. However, since the new scope is inherited from the parent scope, any changes made in the controller ( the parent scope ) will be reflected in the directive scope

<b>Scope : { } -Directive gets a new isolated scope </b>

  * In this case a new scope is created for the directive, but it will not be inherited from the parent scope. This new scope also known as Isolated scope because it is completely detached from its parent scope

  Example-
  ```javascript
    var app = angular.module("test",[]);
app.directive("myDirective",function(){
    return {
        restrict: "EA",
        scope: {},
        link: function(scope,elem,attr){
            // code goes here ...
        }
    }   
 });
  ```
  *AngularJS allows to communicate with the parent scope using some special symbols knows as prefixes. Because of course there are still situations where the directive needs to be able to exchange data with parent scope. The next section is dedicated to Isolated scope and its properties.

<b>"@" Scope:</b>
    
This type of scope is used for passing value to the directive scope. Let's say that you want to create a widget for a notification message:
  
  Example- 
  ```javascript
      app.controller("MessageCtrl", function() {
        $scope.message = "Product created!";
      });
      app.directive("notification", function() {
        return {
            restrict: 'E',
            scope: {
                message: '@'
            },
            template: '<div class="alert">{{message}}</div>'
        }
      });

  ```
  <notification message="{{message}}"></notification>

<b>"=" Scope:</b> 
    * In this scope type, scope variables are passed instead of the values, which means that we will not pass {{message}}, we will pass message instead. The reason behind this feature is constructing two-way data binding between the directive and the page elements or controllers. Let's see it in action
    
    Example-
    ```javascript
    .directive("bookComment", function() {
      return {
          restrict: 'E',
          scope: {
              text: '='
          },
          template: '<input type="text" ng-model="text"/>'
      }
    });

    ```
    <book-comment text="commentText"></book-comment>

<b>&" Scope:</b>

In this scope type we will have a look at how to pass expressions to the directive. In real-life cases, you may need to pass a specific function (expression) to directives in order to prevent coupling. Sometimes, directives do not need to know much about the idea behind the expressions. For example, a directive will like the book for you, but it doesn't know how to do that. In order to do that, you can follow a structure like this

In this directive, an expression will be passed to the directive button via the like attribute. Let's define a function in the controller and pass it to the directive inside the HTML.

Example-

    ```javascript
    .directive("likeBook", function() {
      return {
          restrict: 'E',
          scope: {
              like: '&'
          },
          template: '<input type="button" ng-click="like()" value="Like"/>'
      }
    });

    ```

    ```javascript
    $scope.likeFunction = function() {
      alert("I like the book!")
    }
    ```
This will be inside the controller, and the template will be:
    
<like-book like="likeFunction()"></like-book>

<b>Directive Clean-UP</b>

   * Consider defining clean-up function by registering an event, element.on( ‘$destroy’, …).
   * It is a good practice to remove event listeners once the ‘$destroy’ event is broadcasted, to avoid instances of memory leaks.
   * Listeners registered to scopes and elements are automatically cleaned up when they are destroyed.

   Example-
    element.on('$destroy', function() {
        // Do cleanup work
    });
    OR
    scope.on('$destroy', function() {
        // Do cleanup work
    });
   * There are a few special events that AngularJS emits. When a DOM node that has been compiled with AngularJS's compiler is destroyed, it emits a $destroy event. Similarly, when an AngularJS scope is destroyed, it broadcasts a $destroy event to listening scopes.

   * By listening to this event, you can remove event listeners that might cause memory leaks. Listeners registered to scopes and elements are automatically cleaned up when they are destroyed, but if you registered a listener on a service, or registered a listener on a DOM node that isn't being deleted, you'll have to clean it up yourself or you risk introducing a memory leak.

<b>PreLink, PostLink and Controller Methods of Angular Directives</b>

  the link function has the duty of linking the model to the templates. Link function is the place where AngularJs does the data binding to the compiled templates
  link: function LinkFn(scope, elem, attr, ctrl){}
  There are 4 parameters available to the link function.
  scope : The scope of the directive
  elem : Dom element where the directive is applied
  attr : Collection of attributes of the Dom Element
  ctrl : Array of controllers required by the directive

  Example-

  ```javascript
    var app = angular.module('app', []);
    app.directive('dad', function () {
        return {
            restrict: 'EA',
            template: '<div>{{greeting}}{{name}}</div>',
            link: function(scope,elem,attr){
                scope.name = 'Paul';
                scope.greeting = 'Hey, I am ';
            }
        };
    });

  ```
  The above is the usual way to create a link function inside a directive. However, AngularJs allows to set the link property to an object also. Advantage of having an object is, we can split the link function into two separate methods called, pre-link and post-link
  
  <b>PostLink-</b>
In general we can write the post-link function in two ways-
```javascript
1. var app = angular.module('app', []);
app.directive('dad', function () {
    return {
        restrict: 'EA',
        template: '<div></div>',
        link: function(scope,elem,attr){
            scope.name = 'Paul';
            scope.greeting = 'Hey, I am ';
        }
    };
});

2  var app = angular.module('app', []);
app.directive('dad', function () {
    return {
        restrict: 'EA',
        template: '<div></div>',
        link: {
            post: function(scope,elem,attr){
                scope.name = 'Paul';
                scope.greeting = 'Hey, I am ';
            }
        }
    };
});
```
<b>PreLink-</b>
The signature is of the pre-link function is same as that of a post-link. The only difference between the pre-link and a post-link is the order they gets executed

```javascript
var app = angular.module('app', []);
app.directive('dad', function () {
    return {
        restrict: 'EA',
        template: '<div class="dad">{{greeting}}{{name}}'+
        '<son></son>'+
        '</div>',
        link: function(scope,elem,attr){
            scope.name = 'Paul';
            scope.greeting = 'Hey, I am ';
        }
    };
});
app.directive('son', function () {
    return {
        restrict: 'EA',
        template: '<div class="son">{{sonSays}}</div>',
        link: function(scope,elem,attr){
            scope.sonSays = 'Hey, I am son, and my dad is '+ scope.name;
        }
    };
```
Output- Hey, I am son, and my dad is undefined

We created a son directive and placed inside the dad directive’s template. Since there is no scope specified for the son directive, we assume that all parent directive scope should be available to it. Let’s look at the output tab of the jsFiddle, we can see that the son directive prints like this:

Now let’s analyse what happened. Here, both the dad and son directives have link functions, and both these link functions are post-links. When a directive contains multiple child directives, all of the child directive’s link functions executed first then the parent directive link function. So, in this case, when son directive’s link function executes, the dad directive is still not linked the data to the template. That’s why the son directive outputs the dad’s name as undefined.

This is where the pre-link comes handy. A pre-link function of a directive will get executed before all of its child directives’ link functions

<b>Controller Function-</b>
 
```javascript

var app = angular.module('app', []);
app.directive('dad', function () {
    return {
        restrict: 'EA',
        template: '<div class="dad">{{greeting}}{{name}}'+
        '<son></son>'+
        '</div>',
        controller: function(){
            this.name = 'Paul'
        },
        link: function(scope,elem,attr,ctrl){
            scope.name = ctrl.name;
            scope.greeting = 'Hey, I am ';
        }
    };
});
app.directive('son', function () {
    return {
        restrict: 'EA',
        require: '^dad',
        template: '<div class="son">{{sonSays}}</div>',
        link: function(scope,elem,attr,ctrl){
            scope.sonSays = 'Hey, I am son, and my dad is '+ ctrl.name;
        }
    };
});
```

Changing post-link to pre-link will solve the above problem. However, it’s not a best practice to create pre-link functions whenever we introduce a child directive. Assume, if instead of a child directive, what if we want to share some data to another directive applied to the same DOM element ?

Directive’s controller is designed for that. A controller is a place where directive can define it’s public API.
Order of Execution-
Controller gets executed first
Pre-link gets executed next
Post-link gets executed last
