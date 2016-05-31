# Angular
  * Angular runs on the browser, meaning an Angular app can be a server-less static site.
  * Two way data binding: you can bind data to an input and Angular will automatically update what is bound to that on your Angular side. Angular magic! TBC
  * Angular is not technically MVC, more MVVM (model | view model | view), with the view model being the controller

## Directives
  * Angular's custom HTML tags which go into your HTML to link up an Angular app.
  * `ng-app="*appName*"` - specify the app for your HTML (located in HTML tag).
  * `ng-controller="*controller-name* as ctrl"` - specify the controller to be used (located in a div).
  * `ng-click="ctrl.*expression*"` - specify the controller expression that should be accessed when a click event happens.
  * `ng-repeat="*object* in ctrl*collection* {{ *object*.*parameter* }}` - iterate over a collection of items (in conjunction with mustache tags).
  * `ng-model="*parameter-name*"` - applied to HTML tags(such as inputs) passes the value of the tag as a parameter (in conjunction with event based directives such as `ng-click="ctrl.*expression*"`).

## Angular modules
  * Everything in Angular is split into modules, which are similar to classes in OOP.
  * Angular modules need to be included in a script tag within the HTML file.
  * In an app.js file, you can declare a top-level module for your app: `var *appName* = angular.module('**appName', [])`
  * The array that is currently empty can be used to inject various dependencies the app. When not empty, it contains the list of modules the app depends on.
  * Use the `ng-app="*appName*"` directive in your HTML tag to wire the module into the app page.

## Angular controllers
  * Angular controllers can be used to keep track of a ViewModel. They hold the state of an app.
  * Angular controllers need to be included in a script tag within the HTML file.
  * A controller should sit in it's own file, such as `controllers.js`. An app which requires many controllers should have the controllers split out into separate files.
  * The `.controller` function is used to attach a controller module to an application root module, name the controller module, and set up the controller in a callback provided to the method(TBC):
  ```
  *appName*.controller('*ControllerName*', function(){
    //controller code
    })
  ```
  * Controllers are specified in HTML with the `ng-controller="*controller-name* as ctrl"` directive placed within div tags. Multiple controllers can be used for multiple divs across a page.
  * The directive for controllers requires the use of `*controller-name* as controller/ctrl` syntax to instantiate a controller to a variable that can be accessed by the HTML.

### Controller expressions
  * Controller expressions are methods in the controller module, and can be bound to HTML elements through directives such as `ng-click="ctrl.*expression*"`.
  * Where controller modules hold several objects in state, controller expressions can be used to to manipulate the state.
  * Rather than writing controller expressions using 'this' (which is very context-dependent), it is a good idea to specify a variable that points to 'this' and refer to this variable where you would use 'this': `var self = this;`

## Mustache syntax
  * Mustache syntax is quite similar to string interpolation in Ruby, and is used to interpolate values from an Angular app within HTML: `{{ ctrl.*function* }}`.

### Model directive
  * The `ng-model="*parameter*"` is placed on HTML elements to assign the value of the element to a parameter that can be passed back to the controller. For example `<input type="text" ng-model="parameter">` would assign the value of the input tag to `parameter`. This can used in conjunction with other directives to pass the value as a parameter to a controller expression, such as: `<button ng-click="ctrl.someExpression(parameter)">Click me</button>`.

## Angular models
* As with pure MVC frameworks, the heavy lifting of Angular apps can be pushed into the model.
* The parts of Angular apps that do heavy lifting are referred to as services, and come in two forms: factories and services. The are similar in the way they are built, but they are different in the way they are used:
  * Factories are used for building models.
  * Services are used for anything else, like connecting to APIs.

### Factories
* Factories are defined in their own file (`*ModelName*Factory.js`) and are injected into the controller in an array, which is the callback to the controller. The name of the factory is passed as a string as the first element, with the second element as a function with the factory passed as a parameter:
```
*appName*.controller('*ControllerName*', ['*FactoryName*', function(*FactoryName*) {
  //controller code
  }]);
```
* Factories are specified as modules of an app, and contain any functions the factory needs to complete it's logic:
```
*appName*.factory('*FactoryName*', function() {
  //factory code
  });
```
* Functions in factories can be attached to prototypes, or assigned to variables.
* Factories are called from controllers using the `new` keyword, passing in any parameters. For example: `*array-name*.push(new *FactoryName*(*parameter*))`

### Services
* Services can be used for other functions than building objects, such as retrieving data from APIs.
*
