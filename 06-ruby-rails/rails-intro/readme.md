# Intro to Rails

## Objectives

- principles of rails
- creating an app
- rails file structure
- rails db config
- model generators
- db migrations
- controllers
- rails routes
- views / erb
- view helpers


## Principles of Rails

1. **DRY** - keep your code DRY and use concise, consistent code.
2. **Convention over configuration** - Rails is built using sensible defaults, which speeds development and means that there is less code to maintain.


Rails uses (and for the most part, forces you to adhere to) an **MVC** architecture. We used MVC when creating Express applications.


**Model** - Active Record Class Objects that we use to interact with Postgresql

**View** - The view is the Presentation layer. It's what the user sees and interacts with, essentially the web pages. The HTML, CSS, and front-end JavaScript.

**Controller** - The controller will make decisions based on the request and then control what happens in response. It controls the interaction with our models and with our views.

Many Rails users say that most of your business logic should go in the Model, not in the controller.

![MVC Diagram](http://elibildner.files.wordpress.com/2012/06/screen-shot-2012-06-05-at-2-12-18-am.png)


More info about Rails: [http://rubyonrails.org/](http://rubyonrails.org/)

## Create a Rails App

Basic creation of an app is very simple:

```bash
rails new name_of_the_app
```

If we want to use a different database (such as PostgreSQL) we need to specify the database using the `-d` flag followed by the database. By default, Rails uses SQLite, which is unideal for web applications deployed to ephemeral file systems. We'll specify `postgresql` for our Rails apps.

```bash
rails new name_of_the_app -d postgresql
```

***SPECIAL NOTE FOR UBUNTU/DEBIAN USERS***

You might need to install libpq-dev and build-essential:

```bash
sudo apt-get install libpq-dev build-essential
```


## Rails File Structure

The main directory that we'll be working in is the `app` directory which contains our `models`, `views`, and `controllers`.

![Rails File Structure](http://i.imgur.com/whOL4DQ.png)

More info: [rails guides - getting started](http://guides.rubyonrails.org/getting_started.html#creating-the-blog-application)

## Database config

The configuration for the database can be found in `(Your project name)/config/database.yml` This is where you can find the name of your database, and change database options.

***NOTE FOR UBUNTU/DEBIAN USERS***

You might need to specify the host, user and password as well. Just add the following to `config/database.yml`.

```yaml
host: localhost
user: YOUR USERNAME HERE
password: YOUR DATABASE PASSWORD HERE
```

To create your database.

```bash
rails db:create
```

## Start a server

To start the server we just type

```bash
rails server
```

This will start a server on port 3000.

## Models / ORM / Active Record

Rails provides a tool called Active Record, which is an ORM (Object Relational Mapper) that maps database tables to object-oriented models.

**Example**

To create a Tweet model with the following attributes:

* username - string (varchar)
* content - text

```
touch app/models/tweet.rb
```
Contents of the file:
```
class Tweet < ActiveRecord::Base
  # AR classes are singular and capitalized by convention
end
```

#### Create the table:

```
rails generate migration 
```

Look at the file that got generated:
```
ls -la db/migrate
```

Look inside the generated file:
```
class Articles < ActiveRecord::Migration
  def change
  end
end
```

Inside the `change` method add your table creation code:

```
create_table :articles do |t|
  t.string :title
  t.text :text
  t.timestamps
end
```

more info: [Rails Guide - Active Record](http://guides.rubyonrails.org/active_record_basics.html)

`rails db:create` automatically creates your databases.
Here are some other `rake` commands you'll want to know about for database management.

```bash
rails db:drop # drop database
rails db:migrate # run migrations
rails db:rollback # rollback one migration
rails db:rollback STEP=n # rollback 'n' migrations
```


## Create a controller

`controllers` and the `actions` contained within are the starting point for the back-end code that will be executed when a user visits a particular page/URL.

Create a controller called "MainController" in the file `app/controllers/main_controller.rb`. To create actions we simply define methods inside of the controller like this.

```ruby
class MainController < ApplicationController

  def index
  end

  def about
  end

end
```


## Routing

Routing is used to route URLs to specific controllers/actions. So when a user types in `/about` we want it to go to the about action of the main controller. To specify this we use the `#` symbol so for our about action it'd be `main#about`.

Routes consist of an HTTP verb and a path. `GET /about` is not the same as `POST /about`

Routes are contained in the `config/routes.rb` file.

The syntax of a routes.rb file is a ruby DSL (domain specific language)

The "magic" features of the ruby language allow us to write subsets of functionality that make it look like we have language-level features For example `=` at the end of a method name is assignment.


To list all routes you can run the following command:

```bash
rake routes
```

Inside of config/routes.rb:
```ruby
  root 'main#index'
  get 'about' => 'main#about'
```

* **root** - A special route known as the "root route". Every app only has one root route which is used for the home page of the site, AKA what will display when we go to: `http://localhost:3000`
* **get** - get defines a new `GET` route. Any time you go to a url by typing it into the URL bar it is accessing a `GET` route. Defining routes is simply the url they will type followed by a hash-rocket (`=>`) that points at the controller#action you want it to execute (`main#about`).


###More Routing Examples

```ruby
# a single route to a single controller#action
get 'contact' => 'main#contact'

# same as above, only different syntax
get 'contact', to: 'main#contact'

# similar to above, only with a URL parameter
get 'users/:id', to: 'users#show'

# similar to above, only changing the name of the path helper
get 'users/:id', to: 'users#show', as: 'profile'

# resources routing, used to quickly declare RESTful routes for a resource
resources :photos

# resource routing, using `only` to define the specific RESTful routes
resources :photos, only: [:index, :show]

# resource routing, using `except` to omit RESTful routes
resources :photos, except: [:destroy]

# nested routes
resources: :posts do
  resources :comments
end
```

Note that there are more examples for customizing routes in the `config/routes.rb` file, as well as the [Rails Routing documentation](http://guides.rubyonrails.org/routing.html). Note that there is a "Rails way" for routing that makes your life easier.

---

##Views

By default, actions in rails will render a view named `ACTION_NAME.html.erb` in the `views/CONTROLLER_NAME` directory.

For example, the actions we defined above will load `views/main/index.html.erb` and `views/main/about.html.erb` respectively.

However, we can manually render a view by using the `render` method, if needed. Example:

```ruby
class MainController < ApplicationController

  def about
    render :about
  end

end
```
For rendering text, JSON, other templates, etc., you can take a look at the [Rails Documentation on creating responses](http://guides.rubyonrails.org/layouts_and_rendering.html#creating-responses). Trust us, it's good.


## ERb

Rails uses a templating engine called ERb (Embedded Ruby). It allows us to mix HTML and ruby code to create dynamic templates. It supports the majority of the major components of the ruby language.

To designate ruby code we use "magic tags" `<% #ruby code goes here %>`. Any code between those tags will be executed on the server before the HTML content is served to the user. If you want the result of the code to output you add a `=` inside the tag like this: `<%= 5+5 %>` would insert the number "10" into the HTML.

**Example**

```erb
<% (1..10).each do |i| %>
<%= i %><br>
<% end %>
```

Notice only the middle line of code has an equal sign (`=`). This is because this is the only line that needs to putput anything. The each loop and end tag are just used for control flow.

This could would output the following HTML:

```html
1<br>
2<br>
3<br>
4<br>
5<br>
6<br>
7<br>
8<br>
9<br>
10<br>
```

This HTML is then sent to the user's web browser to be rendered.

##Passing data from controllers to views

**Inside a controller action**

```ruby
def index
  @taco = "Hello instance taco!"
  @array = [1,2,3]
end
```

**Inside a view**

```erb
<%= @taco %>
<%= @array.inspect %>

<% @array.each do |item| %>
<%= @array %>
<% end %>
```

#### ruby variables vs. instance variables

Note that we can pass data from a controller action to a view by defining the variables as instance variables. This is required because instance variables only exist in the action. By declaring them as instance variables, the variables are passed to the view.

More info here: [rails guides layouts and rendering](http://guides.rubyonrails.org/layouts_and_rendering.html)


###Handy Methods for Views

Rails provides a lot of helper methods, most handily `link_to` and `form_for`, as well as methods that produce the links. Note that we can override the names of these helpers by using `as:` when creating routes.

```ruby
# link helpers
tweets_path
tweet_path(tweet)
tweet
new_tweet_path
edit_tweet_path(tweet)
```
---

* [Rails Routing Path/URL Helpers](http://guides.rubyonrails.org/routing.html#path-and-url-helpers)

```erb
<%= link_to "Edit Tweet", edit_tweet_path(tweet), class: 'btn btn-default' %>

<%= form_for @tweet do |t| %>
  <div>
  <%= t.label :content %>
  <%= t.text_area :content %>
  </div>

  <div>
  <%= t.label :username %>
  <%= t.text_field :username %>
  </div>

  <%= t.submit %>
<% end %>
```


* [Rails Form Helpers](http://guides.rubyonrails.org/form_helpers.html)
* [Rails Form Methods](http://guides.rubyonrails.org/form_helpers.html#how-do-forms-with-patch-put-or-delete-methods-work-questionmark)



Note that if we create a form helper on an edit page, the helper automatically makes assumptions about the form. One of these assumptions is to provide a hidden `_method` field that describes the method that should be used on submission. This is the Rails workaround to sending `PUT` and `DELETE` requests!


Note that we can add a `method` attribute to links as well, using a URL helper. Here's an example.

```erb
<%= link_to "Delete Tweet", tweet_path(tweet), method: :delete, class: 'btn btn-danger' %>
```

Note that this is made possible by a piece of JavaScript called `rails.js` running on the page.

#### Further Topics:

##### Generators

Generators are move auto-magic rails functionality that helps spin up an app as quickly as possible. It looks flashy but can be confusing.

Rails includes a few generators which are command line tools used to create files for us. This automates the repetitive task of creating some of the more common files we'll need to make when building a rails app. To run a generator we type `rails generate` or...

```bash
rails g
```

...for short

The two that we will be using regularly are:

* `rails g migration`
* `rails g model`

We will touch on actual usage of both of these.

More info: [Rails guides - command-line tools](http://guides.rubyonrails.org/command_line.html#rails-generate)

---

##Additional Resoures

* [Getting Started](http://guides.rubyonrails.org/getting_started.html)
* [Creating Migrations](http://edgeguides.rubyonrails.org/active_record_migrations.html)
* [Rails Routing](http://guides.rubyonrails.org/routing.html)
* [Form Helpers](http://guides.rubyonrails.org/form_helpers.html)
* [Rails API guide](http://api.rubyonrails.org/)
