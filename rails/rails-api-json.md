***UNDER CONSTRUCTION***

# Setting up a Rails API with an Angular frontend on separate repositories

## Index
* [Setting up a Rails API to output JSON](#setting-up-a-rails-api-to-output-json)
* [Setting up a AngularJS front end to consume JSON](#setting-up-a-angularjs-front-end-to-consume-json)
* [Connecting the API to Angular](#connecting-the-api-to-angular)


# Setting up a Rails API to output JSON
Create a directory for the Rails backend, then follow the below steps:

  * `gem install rails-api` to install Rails API
  * `rails-api new *app-name* -d postgresql -T` creates an app using a PostrgreSQL database (defined by `-d postgresql`), and turns off the built in Rails testing suite (defined by `-T`)
  * `bin/rake db:create` will create the database. Sometimes it is necessary to run `bin/rake db:create RAILS_ENV=test`

### Testing
  * Since the Rails testing suite is turned off, the `rspec-rails` gem will need to be added to the gemfile in the test group, and bundle will need to to be run.
  * Run `bin/rails g rspec:install` to install rspec
  * Tests can be written in rspec to test the output of JSON at each route. Examples below of GET and POST request tests:
  ```
  require 'rails_helper'

  describe "Posts"do
    it 'is created with a name attribute' do
      post = Post.create(name: "Post name")
      get '/posts'
      json = JSON.parse(response.body)
      expect(response).to be_success
      expect(json['posts'].length).to eq(1)
    end

    it 'creates a post from an API request' do
      post '/posts', { name: "Post name" }
      expect(response).to be_success
      expect(Post.last.name).to eq 'Post name'
    end
  end
  ```

### Controller
  * Controllers can be built in much the same way as controllers in a standard Rails app, with the addition of the line `render json: @*variable*` to output the objects assigned to a variable as JSON. This will then be accessible from the specified route. An example for outputting all posts as JSON:
  ```
  def index
    render json: Post.all
  end
  ```

### Routes
  * As this Rails is will be purely used as an API, unnecessary GET routes (`new, edit`) can be removed when specifying routes in `routes.rb`. This can be done by like so: `resources :posts, except: [:new, :edit]`

# Setting up a AngularJS front end to consume JSON
Create a separate directory for the JSON frontend, then follow the below steps:

## Setting up an Angular app with NPM & Bower, using Protractor/Karma for testing
* NPM is node package manager, and is used for managing backend packages.
* Bower is a package manager that is similar to node, and is used for managing frontend packages.
* Protractor is a testing framework used for end-to-end feature testing in Angular [Protractor cheat sheet]('https://github.com/makersacademy/course/blob/master/pills/protractor.md').
* Karma is a testing framework used for unit testing in Angular.
* Run `npm init`, hitting return on each question to accept the defaults. This will initialise NPM and create a `package.json` file (similar to a Ruby gemfile).
* Run `npm install bower -g --save-dev`This will install Bower for the project. The `--save-dev` part of the command saves bower in the `package.json` as a devDependency (dependencies only required for the dev environment).
* Run `bower init`, hitting return on each question to accept the defaults. This will initialise Bower and create a `bower.json` file.
* Make an app directory and cd into it.
* Create a `.bowerrc` file in the root directory.
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
* Run `karma init`
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

# Connecting the API to Angular
  * In your Rails project, add the following to the gemfile then run bundle: `gem 'rack-cors', :require => 'rack/cors'`
  * Open the `application.rb` file within the `config` folder, then add the following code within the `Application` class (when being pushed to production, the `origins` URL will need to be updated):

  ```
  config.middleware.insert_before 0, "Rack::Cors" do
    allow do
      origins 'http://localhost:8080'
      resource '*', headers: :any, methods: [:get, :post, :delete]
    end
  end
  ```
  
  * Within your Angular frontend, begin any calls to API URLs with `http://localhost:8080`
  * After setting up your backend and frontend, follow the below steps to check they are connected:
  * Run your Rails server with `rails s` within your Rails directory
  * Run your Angular http-server with `npm run start`
  * Navigate to http://localhost:8080
  * You should now be able to interact with your Angular app, and any API requests should be sent to your Rails API
