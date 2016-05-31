# Rails Project Set Up
  * `gem install rails` installs the Rails gem

## General Rails Commands
  **commands containing 'g' are for generating. The same commands can run with 'd' to destroy anything**
  * `bin/rails s` will start the server
  * `bin/rake db:create` will create a database
  * `bin/rake routes` will show a list of different routes created for resources
  * `bin/rails g controller controller-name` will generate a controller file in `app/controllers` and associated files
  * `bin/rails g model *model-name* *property*: *property-type*` creates models
  * `bin/rails g migration *AddParameterToModel* *parameter*:*format*` creates a migration


## Creating An App
  * `rails new app_name` with the your app name on place of 'app_name' creates a basic app
  * `rails new app_name -d postgresql -T` with the your app name on place of 'app_name' creates an app using a PostrgreSQL database (defined by `-d postgresql`), and turns off the built in Rails testing suite (defined by `-T`)
  * `bin/rake db:create` will create the database. Sometimes it is necessary to run `bin/rake db:create RAILS_ENV=test`

### Testing
  * since the Rails testing suite is turned off, the following gems will need to be added to the gemfile in the test group, and bundle will need to to be run:
    - `gem rspec-rails`
    - `gem capybara`
  * `require 'capybara/rails'` will need to be added in `spec/rails_helper.rb` below the other require statement to let you use Capybara in your testing environment
  * run `bin/rails generate rspec:install` to install rspec

## Routing
  * Rails puts routing concerns in different files to separates routing concerns from controller action concerns. TBC
  * `config/routes.rb` contains all the routes
  * `app/controllers/*controller name*_controller.rb` contains all the controller methods
  * `get 'restaurants' => 'restaurants#index'` will add individual routes, in this case creating a /restaurants index page
  * `resources :resource-name` will make it possible to create all the commonly used routes associated with a resource
    * `bin/rake routes` will show a list of the different routes created by the for the resource, which follow RESTful conventions. There will be 5 columns, which are as follows:
      - Prefix: special prefixes we can use to refer to routes in our Rails code
      - Verb: the HTTP verb used to access a route when using the specific URI pattern
      - URI Pattern: the pattern used to access a specific route, including any optional formatting and ID variables
      - Controller Action: the action taken in the controller when this route is accessed (includes CRUD methods)

## Controllers
  * controllers are similar to the methods found in a Sinatra server file. They contain methods whose names directly correlate to the Controller Actions from `bin rake/routes`
  * `bin/rails g controller *controller-name*` will generate a controller file in `app/controllers` and associated files

## Views
  * view files for each resource sit within `app/views/*resource-name*/*view-name*` and have a double file extension dependent on the templating language (for example .html.erb or html.haml)
  * views are automatically wrapped in a layout view, which is located at `app/views/layouts/application.html.erb`

## Models
  * models hold the logic for your app, the same as Sinatra
  * `bin/rails g model *model-name* *property*: *property-type*` creates models. Multiple properties can be included when creating a model, for example:
    `bin/rails g model restaurant name:string rating:integer`
  * the above command creates a model, telling the app the properties of that model, as well as creating a migration telling rake how to build the database for the model
  * where Data Mapper was used when we created apps in Sinatra, the Rails gem ActiveRecord creates objects to be stored in databases. Unlike Sinatra, where properties are defined in the model, Rails and ActiveRecord define properties in migrations. Item ID's are created automatically
  * important to note is that in the model is singular (eg. restaurant), but the controller makes references in the plural (restaurants)
  * after creating any models with properties, `bin/rake db:migrate should be run`

## Migrations
  * migrations are ways of making changes to databases in Rails, and are commands to rake to run the SQL commands required for these changes. They are also useful for keeping a log of all changes made to the database, and make it possible to roll back on previous changes
  * migration commands look like `bin/rails g migration *AddParameterToModel* *parameter*:*format*`

## Associations
  * the first step for setting up associations is to add nested routes for the relevant resources in `routes.rb`, for example:
  ```
  resources :*resource-name* do
    resources :*nested-resource name*
  end
  ```
  * after doing, the relevant routes should appear when running `bin/rake routes`
  * generate a controller for the resource with a Rails command from the command line
  * generate a models for the resource with a Rails command from the command line
  * run a migration to update the database with the appropriate fields for the association, for example:
    `bin/rails g migration Add*resource-1*RefTo*resource-2* *resource-1*:*parameter*`
  * the models will also need to be updated to add the relevant associations, such as `has_many :*resource*`

## Rails forms
  * Rails has helpers for creating HTML forms, since they are a common way to get data in web apps
    - example `form_for` helper for haml:
    ```
    <%= form_for @*model* do |f| %>
      <%= f.label :*parameter* %>
      <%= f.*input-type* :*parameter* %>
      <%= f.submit %>
    <% end %>
    ```
    - example `form_for` helper for erb:
    ```
    = form_for @*model* do |f|
      = f.label :*parameter*
      = f.*input-type* :*parameter*
    = f.submit
    ```
  * permit must be used in Rails 3.2 onwards when getting params from a form. This stops forms from blindly accepting params passed in via a form, and will only accept the specified params
  * to use permit, create a private method such as `model-name_params` with the code `params.require(:*model*).permit(:*any-parameters*)` then pass this method as an argument in to methods such as `create` when params are taken from forms

## Rails links
  * creating links to pages dynamically by ID can be done by using/adapting the following haml/erb code and method:
    - haml:
      `<%= link_to *model*.name, *model*_path(*model*) %>`
    - erb:
      `= link_to *model*.name, *model*_path(*model*)`
    - controller method:
      ```
      def show
        @*model* = *Model*.find(params[:id])
      end
      ```
  * the above method is called when going to the URL `/*model*/*model_id*`, using the ID parameter in the URL to look up the object in the database
  * any Rails links to routes found in `bin/rake/routes` without a prefix must have their method specified in the link, for example `= link_to "Delete #{*resource*.name}", *resource*_path(*resource*), method: :delete`

## Validation
  * `validates :*parameter*, `

## Testing

### Application helpers
  * create `spec/helpers/application_helper.rb`
  * require this file within `spec_helper.rb`
  * add the following on a new line after `RSpec.configure do |config|` within `spec_helper.rb` : `config.include ApplicationHelper`
  * wrap all helpers in the `application_helper.rb` file in a module named `ApplicationHelper`
  * ensure `application_helper.rb` is required in each test with helpers

### Unit testing validation
  *
