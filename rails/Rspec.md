osx 10.10.1 | Jan 14, 2015

##Install
Add `rspec-rails` to the `Gemfile` (inside `:development` and `:test` groups) and run bundler:  
```ruby
group :development do                   
    gem 'rspec-rails'
end
```


Initialize the `spec/` directory (where specs will reside) with:
```ruby
rails generate rspec:install
```
This adds the following files which are used for configuration:
- `.rspec`
- `spec/spec_helper.rb`
- `spec/rails_helper.rb`
Check the comments in each file for more information.  

##Useage

Use the rspec command to run your specs:

```ruby
bundle exec rspec
```

Specs can also be run via `rake spec`, though this command may be slower to start than the `rspec` command.

In Rails 4, you may want to create a binstub for the `rspec` command so it can be run via `bin/rspec`:

```ruby
bundle binstubs rspec-core
```

####Generators
Once installed, RSpec will generate spec files instead of Test::Unit test files when commands like `rails generate model` and `rails generate controller` are used.

You may also invoke RSpec generators independently. For instance, running `rails generate rspec:model` will generate a model spec. For more information, see list of all [generators][rspec generators].  


###Matchers

Several domain-specific matchers are provided to each of the example group types. Most simply delegate to their equivalent Rails' assertions.

####`be_a_new`

Available in all specs
Primarily intended for controller specs
expect(object).to be_a_new(Widget)
Passes if the object is a Widget and returns true for new_record?

####`render_template`

Delegates to Rails' assert_template
Available in request, controller, and view specs
In request and controller specs, apply to the response object:

expect(response).to render_template("new")
In view specs, apply to the view object:

expect(view).to render_template(:partial => "_form", :locals => { :widget => widget } )
####`redirect_to`

Delegates to assert_redirect
Available in request and controller specs
expect(response).to redirect_to(widgets_path)  

####`route_to`

Delegates to Rails' assert_routing
Available in routing and controller specs
expect(:get => "/widgets").to route_to(:controller => "widgets", :action => "index")  

####`be_routable`

Passes if the path is recognized by Rails' routing. This is primarily intended to be used with not_to to specify standard CRUD routes which should not be routable.

```ruby
expect(:get => "/widgets/1/edit").not_to be_routable  
```

####`have_http_status`

- Passes if `response` has a matching HTTP status code
- The following symbolic status codes are allowed:
    * `Rack::Utils::SYMBOL_TO_STATUS_CODE`
    * One of the defined `ActionDispatch::TestResponse` aliases:
       + :error
       + :missing
       + :redirect
       + :success
- Available in controller, feature, and request specs.  
In controller and request specs, apply to the response object:  

```ruby
expect(response).to have_http_status(201)
expect(response).not_to have_http_status(:created)
```

In feature specs, apply to the page object:  

```ruby
expect(page).to have_http_status(:success)
```

###`rake` tasks

Several rake tasks are provided as a convience for working with RSpec. To run the entire spec suite use `rake spec`. To run a subset of specs use the associated type task, for example `rake spec:models`.

A full list of the available rake tasks can be seen by running `rake -T | grep spec`.
















###Stubbing with Mocha (not recomended)
`gem 'rspec-rails-mocha'`









[rspec-rails]: https://github.com/rspec/rspec-rails
[rspec generators]: https://www.relishapp.com/rspec/rspec-rails/docs/generators