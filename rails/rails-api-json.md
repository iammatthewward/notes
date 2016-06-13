# Setting up a Rails API to output JSON

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
