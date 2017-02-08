#Rails Authentication and 1:M Associations

##Objectives

* Create users and store their passwords securely
* Enable the ability to authenticate users and store sessions once logged in
* Utilize filters and validations in Rails
* Establish 1:M relationships

Remember all that hassle of setting up authentication in Node? Rails makes it easy.

##Create a new project

You should know how to do this now. If not, see notes from Intro to Rails.

##Create a user model

We need to first start creating a user model that has a username/email field and a `password_digest`. Note that you **have** to name the field this.

```
rails g model user email password_digest
rails db:migrate
```

###Add some validations
[http://guides.rubyonrails.org/active_record_validations.html](http://guides.rubyonrails.org/active_record_validations.html)

**app/models/user.rb**

```ruby
validates :email,
presence: true,
uniqueness: {case_sensitive: false}
```

Note that we're only checking for presence and uniqueness of the email. If we want to validate the format of the email, we can use an Regex like so

```ruby
validates :email,
presence: true,
uniqueness: {case_sensitive: false},
format: /\A[\w+\-.]+@[a-z\d\-.]+\.[a-z]+\z/i
  ```

or we can use [this gem](https://github.com/balexand/email_validator) for a more comprehensive comparrison.

###Add password hashing

* Add `has_secure_password` to the user model
* uncomment `gem 'bcrypt'` on your Gemfile and run the bundler

Now that we have `has_secure_password`, Rails gives out a password setter that we can use in validations even though we don't have a password field in our database.

```ruby
validates :password, length: { in: 8..72 }, on: :create
```

## Register
We now have everything we need to create a user, and we can do so using Rails Console.

```rb
User.create!(email: 'paul@gmail.com', password: '12345678')
```

But that's no use to our users, so let's add a registration page.

###Add the register page
Let's create a users controller to handle registering.

```
rails g controller users new
```

Manually add a controller action to `create` - as it won't have a view.

###Add the routes

```ruby
get "register" => "users#new"
post "register" => "users#create"
```

### Create a Registration Form

```erb
<h1>Register</h1>

<%= form_for @user, url: register_path do |f| %>
  <%= f.email_field :email, placeholder: "Enter your email" %>
  <%= f.password_field :password, placeholder: "Enter your password" %>
  <%= f.password_field :password_confirmation, placeholder: "Please confirm it" %>
  <%= f.submit "Register" %>
<% end %>
```

### Controller Actions
We need to fill up our controller to serve these requests.
```ruby
def new
  @user = User.new
end

def create
  @user = User.new(user_params)
  if @user.save
    flash[:success] = "Account Created. Please Login"
    redirect_to root_path
  else
    render :new
  end
end

private

def user_params
  params.require(:user).permit(:email, :password, :password_confirmation)
end
```
We should now be able to register a new user, next we want to be able to log them in.

## Logging In & Out (Sessions)
Using had_secure_password, we gain an authenticate method that we can call on any instance of a user and compare a password. It will return the user if the password is valid else false, for example...

```ruby
User.find_by_email('paul@gmail.com').try(:authenticate, '123')
```

We can add a class method to our User model to wrap this up and find and return a user only if it exists and the password is valid - based on the passed in params from the controller.

###Add a helper method to the class

**app/models/user.rb**

```ruby
def self.find_and_authenticate_user(params)
  User.find_by_email(params[:email]).try(:authenticate, params[:password])
end
```

###The finished User model

```ruby
class User < ActiveRecord::Base
  validates :email,
  presence: true,
  uniqueness: {case_sensitive: false}

  validates :password,
  length: { in: 8..72 },
  on: :create

  has_secure_password

  def self.find_and_authenticate_user(params)
    User.find_by_email(params[:email]).try(:authenticate, params[:password])
  end
end
```

### Add the login pages
Let's create a session controller to handle logging in/out. We'll organize this by calling the controller `sessions`, because in reality, we're creating and destroying sessions on login and logout.

```
rails g controller sessions new
```

manually add actions `create` and `destroy` - they won't have views.

###Lets create some routes

```ruby
get "login" => "sessions#new"
post "login" => "sessions#create"
delete "logout" => "sessions#destroy"
```

### Lets generate a form

```erb
<h1>Login</h1>

<%= form_for :user do |f| %>
  <%= f.email_field :email, placeholder: "Enter your email" %>
  <%= f.password_field :password, placeholder: "Enter your password" %>
  <%= f.submit "Login" %>
<% end %>
```

Wait, why are we using the symbol? [See this StackOverflow answer](http://stackoverflow.com/questions/957204/instance-variable-vs-symbol-in-ruby-on-rails-form-for)

###Authenticate
Authenticate the user on `sessions#create`

```ruby
def create
  user = User.find_and_authenticate_user(user_params)

  if user
    session[:user_id] = user.id
    flash[:success] = "User logged in!!"
    redirect_to root_path
  else
    flash[:danger] = "Credentials Invalid!!"
    redirect_to login_path
  end
end

def destroy
  session[:user_id] = nil
  flash[:success] = "User logged out!!"
  redirect_to root_path
end

private

def user_params
  params.require(:user).permit(:email, :password)
end
```

###Add current User capabilities

```ruby
class ApplicationController < ActionController::Base
  # Prevent CSRF attacks by raising an exception.
  # For APIs, you may want to use :null_session instead.
  protect_from_forgery with: :exception

  def is_authenticated
    unless current_user
      flash[:danger] = "Credentials Invalid!!"
      redirect_to login_path
    end
  end

  def current_user
    @current_user ||= User.find_by_id(session[:user_id])
  end

  helper_method :current_user
end
```

The final line tells rails that we want the current_user method to be available inside of our views not just our controllers.

###Adding Flash Messages

The `flash` hash is accessible in every Rails controller and view. To access it, we'll need a way to iterate through the hash and print out the keys and values. The best way is to create a partial and include it on the layout (so it'll be on every page).

Partials have to start with an underscore in Rails. We can render the partial by using the `render` helper.

With a partial at **app/views/partials/_flash.html.erb**

```erb
<%= render "partials/flash" %>
```


**_flash.html.erb**

```erb
<% flash.each do |key, value| %>
  <div class="alert alert-<%= key %>">
    <%= value %>
  </div>
<% end %>
```

###Protect a controller or action

To protect a controller or action we can simply add a `before_action :is_authenticated` at the top. For example, let's add a profile page that you can only see when logged in.

```ruby
class UsersController < ApplicationController
  before_action :is_authenticated, only: [:profile]

  ...

  def profile
  end
```

Let's add a view.

**users/profile.html.erb**

```erb
<h1>Hello <%= current_user.email %></h1>
```

finally add a route to get there. We can also make it the default (root) route of our site.

```ruby
  root 'users#show'
  get "profile" => "users#show"
```

##Adding 1:M relationships with another model

Let's first add another model to relate to the user. In order for the user to have many pets, we can create the model by including the model name and `references` as the type.

```
rails g model pet name user:belongs_to
```

This will make the following migration, which will include a userId in the pet model.

```ruby
class CreatePets < ActiveRecord::Migration
  def change
    create_table :pets do |t|
      t.string :name
      t.references :user, index: true, foreign_key: true

      t.timestamps null: false
    end
  end
end
```

Then, make sure to migrate and include the associations in each model.

```
rails db:migrate
```

**models/user.rb**
```ruby
class User < ActiveRecord::Base
  has_many :pets

  # ...
end
```

**models/pet.rb**
```ruby
class Pet < ActiveRecord::Base
  belongs_to :user
end
```

Now try testing in the Rails console

```ruby
User.first.pets

User.first.pets.create(name: 'Fido')

Pet.all

Pet.first

Pet.first.user
```

Once we create the CRUD controller for our pets, we can easily associate models with the logged in user, through the current_user method.

```ruby
current_user.pets

current_user.pets.create(name: 'Fido')

current_user.pets.find_by(id: params[:id])
```

The last example is useful for stopping a user accessing a pet they didn't create. The query is effectively looking for a Pet with a particular id and also a particular user_id.

## Relationship Constraints
Rails enables us to add additional properties to our associations. For instance we can tell rails to automatically delete all pets, if we delete their owner.

**models/user.rb**
```ruby
class User < ActiveRecord::Base
  has_many :pets, dependent: :destroy

  # ...
end
```

If you don't want to delete the associated method but you want it to always have an owner then you can use a before_destroy action to reassign it, for example...

**models/user.rb**

```ruby
  has_many :pets
  before_destroy :re_home

  private

  def re_home
    # you can be more specific but here we just randomly pick another user, whose is not this one. If no user is found only then do we destroy the pets
    new_owner = User.where.not(id: id).sample(1).first
    if new_owner
      pets.update_all(user_id: new_owner.id)
    else
      pets.destroy_all
    end
  end
```
### Optional Ownership
Rails 5, will also stop us from creating a record that has a belongs_to association, unless that association exists. For example, we cannot create a Pet without an owner.

We can disable this behavior if we want using the optional flag.

**models/pet.rb**
```ruby
class Pet < ActiveRecord::Base
  belongs_to :user, optional: true
end
```
