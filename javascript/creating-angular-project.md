# Angular
  * Angular runs on the browser, meaning an Angular app can be a server-less static site.
  * Two way data binding: you can bind data to an input and Angular will automatically update what is bound to that on your Angular side. Angular magic! TBC
  * Angular is not technically MVC, more MVVM (model | view model | view), with the view model being the controller

## Index
* [Directives](#directives)
* [Running Angular tests](#running-angular-tests)
* [Angular modules](#angular-modules)
* [Angular controllers](#angular-controllers)
* [Controller expressions](#controller-expressions)
* [Mustache syntax](#mustache-syntax)
* [Model directive](#model-directive)
* [Angular models](#model-directive)
* [Factories vs. services](#factories-vs-services)
* [Factories](#factories)
* [Services](#services)
* [Using a service to get data from an API](#using-a-service-to-get-data-from-an-api)
* [Setting up an Angular app with NPM & Bower, using Protractor/Karma for testing](#setting-up-an-angular-app-with-npm--bower-using-protractorkarma-for-testing)
* [Setting up Protractor](#setting-up-protractor)
* [Create a Protractor config file](#create-a-protractor-config-file)
* [Setting up Karma](#setting-up-karma)
* [Create a Karma config file](#create-a-karma-config-file)
* [Protractor test files](#protractor-test-files)
* [Karma test files](#karma-test-files)
* [Writing and running tests](#writing-and-running-tests)
* [Running tests](#running-tests)

## Directives
  ***Angular's custom HTML tags which go into your HTML to link up an Angular app.***
  * `ng-app="*appName*"` - specify the app for your HTML (located in HTML tag).
  * `ng-controller="*controller-name* as ctrl"` - specify the controller to be used (located in div tag).
  * `ng-click="ctrl.*expression*"` - specify the controller expression that should be accessed when a click event happens.
  * `ng-repeat="*object* in ctrl*collection* {{ *object*.*parameter* }}` - iterate over a collection of items (in conjunction with mustache tags).
  * `ng-model="*parameter-name*"` - applied to HTML tags (such as inputs), passes the value of the tag as a parameter (in conjunction with event based directives such as `ng-click="ctrl.*expression*"`).

## Running Angular tests
  * To run tests (using Protractor and Karma), the following need to be running and open (in their own terminal tab):
    * Run the http server: `npm run start`
    * Run the Selenium server: `npm run webdriver-manager start`
  * In a third terminal tab, run the following to run tests:
    * Run Karma tests: `karma start test/karma.conf.js`
    * Run Protractor tests: `npm run protractor test/protractor.conf.js`

## Angular modules
  * Everything in Angular is split into modules, which are similar to classes in OOP.
  * Angular modules need to be included in a script tag within the HTML file.
  * In an app.js file, you can declare a top-level module for your app: `var *appName* = angular.module('*appName*', [])`
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

### Factories vs. services
[source](http://stackoverflow.com/questions/15666048/angularjs-service-vs-provider-vs-factory)
* When declaring a service as an injectable argument, you are provided an instance of the function.
* When declaring a function as an injectable argument, you are provided with the value returned by invoking the function.
* When using a service it is instantiated for you by Angular (using the 'new' keyword behind the scenes), so you will be adding properties to 'this', and the service returns 'this'. When accessing a service via a controller, the properties defined on 'this' are available to the controller through the service.
* When using a Factory an object is created that you add properties to, and this object is returned.  When accessing a factory via a controller, the properties defined on the object are available

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
* Services have similar basic syntax to a factory:
```
*appName*.service('*ServiceName', function() {
  //service code
  });
```
* Services are meant to be singletons, and so there should only be one service defined per app. This service can be used to do whatever is needed of it.

#### Using a service to get data from an API
* A service can be used to get data from an API, using Angular's $http module to fetch data.
* The below example demonstrates how a service can be used to to get todos from an API and pass them to a todo factory:
```
toDoApp.service('ToDoService', ['$http', 'ToDoFactory', function($http, ToDoFactory) {
  var self = this;

  self.getAll = function() {
    var todos = [];
    _fetchTodosFromApi(todos);
    return todos;
  };

  function _fetchTodosFromApi(todos) {
    $http.get('http://*url*/todos.json')
      .then(function(response) {
        _handleResponseFromApi(response.data, todos)
      }, function(err) {});
  };

  function _handleResponseFromApi (data, todos) {
    data.forEach(function(toDoData){
      todos.push(new ToDoFactory(toDoData.text, toDoData.completed));
    });
  }
}]);
```

## Setting up an Angular app with NPM & Bower, using Protractor/Karma for testing
* NPM is node package manager, and is used for managing backend packages.
* Bower is a package manager that is similar to node, and is used for managing frontend packages.
* Protractor is a testing framework used for end-to-end feature testing in Angular [Protractor cheat sheet]('https://github.com/makersacademy/course/blob/master/pills/protractor.md').
* Karma is a testing framework used for unit testing in Angular.
* Run `npm init`, hitting return on each question to accept the defaults. This will initialise NPM and create a `package.json` file (similar to a Ruby gemfile).
* Run `npm install bower -g --save-dev`This will install Bower for the project. The `--save-dev` part of the command saves bower in the `package.json` as a devDependency (dependencies only required for the dev environment).
* Run `bower init`, hitting return on each question to accept the defaults. This will initialise Bower and create a `bower.json` file.
* Make an app directory and cd into it.
* Create a `.bowerrc` file in the root of the app directory.
* Inside the `.bowerrc` file, put the following code so that bower will install it's components inside of the app directory:
```
{
  "directory": "app/bower_components"
}
```
* Run `bower install angular --save` to install Angular, adding the package to `bower.json` as dependencies (due to the `--save` part of the command).
* Create a `.gitignore` file, adding the `node_modules` and `app/bower_components` directories to the file to stop them being added to the Github repo.
* Create an `app/js` directory to hold all Javascript sub-directories and files.

### Setting up Protractor
* Run `npm install http-server --save` to install http-server and add it to your projects dependencies. HTTP-server is used to allow protractor to to access the app URLs.
* Add the following script within the 'scripts' section of the `package.json` file to run http-server with the `npm run start` command: `"start": "cd app && ../node_modules/http-server/bin/http-server"`.
* Ensure Java is installed: `java -version`. If not, install it.
* Run `npm install protractor --save` to install protractor and add it to your projects dependencies.
* Add the following script within the 'scripts' section of the `package.json` file: `"webdriver-manager": "./node_modules/protractor/bin/webdriver-manager"`. This adds an alias for `webdriver-manager`, a program that lets you install Selenium and a browser driver library.
* Run `npm run webdriver-manager update` to install Selenium and the Chrome browser driver.
* Run `npm install jasmine-spec-reporter --save-dev` to format the tests into colours when run.

#### Create a Protractor config file
* The Protractor config file, and all feature tests, are held in a `/test` directory - make this now.
* Create a `test/protractor.conf.js` file, and put the following code within it:
```
exports.config = {
  seleniumAddress: 'http://localhost:4444/wd/hub',
  specs: ['e2e/*.js'],
  baseUrl: 'http://localhost:8080',
onPrepare: function() {
    var SpecReporter = require('jasmine-spec-reporter');
    jasmine.getEnv().addReporter(new SpecReporter({displayStacktrace: 'all'}));
   }
}
```
* `seleniumAddress` points to the Selenium server URL.
* `spec` points to the directory and file pattern where tests are held.
* `baseUrl` points to the URL of the Angular app.
* Create a `test/e2e` directory to store your tests (as per Angular convention), and create a `test/e2e/app.spec.js` file for your app feature tests.
* Add the follwing script within the 'scripts' section of the `package.json` file: `"protractor": "./node_modules/protractor/bin/protractor"`

### Setting up Karma
* Run `npm install karma --save-dev`
* Run `npm install karma-jasmine karma-chrome-launcher --save-dev`
* Run `npm install jasmine-core --save-dev`
* Run `npm install -g karma-cli`
* Run `bower install angular-mocks --save-dev`
* Run `run karma init`
* Run `npm install karma-spec-reporter --save-dev`

### Create a Karma config file
  * A `karma.conf.js` file will have been generated in the `test` directory when running `karma init`. Open this file up and replace the contents with the below configuration:
```
module.exports = function(config){
    config.set({

      basePath : '../',

      files : [
        'app/bower_components/angular/angular.js',
        'app/bower_components/angular-mocks/angular-mocks.js',
        'app/js/**/*.js',
        'test/unit/**/*.js'
      ],

      autoWatch : true,

      frameworks: ['jasmine'],

      browsers : ['Chrome'],

      plugins : [
              'karma-chrome-launcher',
              'karma-jasmine',
              "karma-spec-reporter"
      ],
      reporters: ["spec"],
        specReporter: {
        maxLogLines: 5,         
        suppressErrorSummary: true,  
        suppressFailed: false,  
        suppressPassed: false,  
        suppressSkipped: true,  
        showSpecTiming: false
      },
    });
};
```

## Writing and running tests
  * Protractor and Karma tests are written slightly differently, as one feature tests and one unit tests. They also need several servers to be running in order to run the tests.

### Protractor test files
  * Protractor test files should all be located within the `test/e2e` directory by convention.
  * Tests are written using Jasmine and Protractor syntax; Jasmine syntax is used for `describe`, `it`, and `expect` statements, with Protractor syntax defining the browser interaction required for feature testing.
  * An example of selectors and commands for Protractor can be found [here](https://github.com/makersacademy/course/blob/master/pills/protractor.md#selecting-elements-in-a-web-page)
  * Example test:
  ```
  describe("app", function() {
    it("should say 'Hello world' on the page", function() {
      browser.get('/');
      expect($$("p").first().getText()).toEqual("Hello world");
    });
  });
  ```

### Karma test files
  * Karma test files are written in pure Jasmine syntax, but as they are unit tests they require any dependencies to be injected in to the test files which Protractor does not.
  * At the top of each test file, within the first describe block, the app module should be required with the line `beforeEach(module('*appName*'));`.
  * A `beforeEach` block should be added before all tests, which injects the controller/factory/service being tested (not forgetting to define the controller/factory/service variable above this block
  * Controller `beforeEach`:
  ```
  beforeEach(inject(function($controller) {
    *variable* = $*controller-name*('*controller-name*');
  }));
  ```
  * Factory `beforeEach`:
  ```
  beforeEach(inject(function(*FactoryName*) {
    *variable* = new *FactoryName*('*any-parameters*');
  }));
  ```
  * Service `beforeEach`:
  ```
  beforeEach(inject(function(_*ServiceName*_) {
    *variable* = _*ServiceName*_;
  }));
  ```

### Running tests
* To run tests (using Protractor and Karma), the following need to be running and open (in their own terminal tab):
  * Run the http server: `npm run start`
  * Run the Selenium server: `npm run webdriver-manager start`
  * In a third terminal tab, run the following to run tests:
  * Run Karma tests: `karma start test/karma.conf.js`
  * Run Protractor tests: `npm run protractor test/protractor.conf.js`
