# 1 to Many Associations

## Objectives

* Implement one to many relationships through models in Rails
* Use the `collection_select` form helper to display a collection of associated items

Let's create a minimal rails app so that we can demonstrate the use of associations inside the rails app.

Rails gives us a few tools to make associations easy inside the controllers and views (in addition to active record)


## Create the app:
```bash
rails new parks -d postgresql
```

## What we need

* Models
  * Park
  * Ranger
* Association
  * Park `has_many` Rangers
  * Ranger `belongs_to` a Park
* Views
  * parks#new
  * parks#show
    * list all rangers with a specific park

  * rangers#new - add/remove rangers select
  * rangers#show

* Controllers
  * parks#new
  * parks#show
    * list all rangers with a specific park

  * rangers#new - get a list of all parks here
  * rangers#show

* Routes
  * We can nest the routes of one resource within the other one.

## Generating models / databse migrations

Review of **Parks**

```bash
rails g model park name description:text
```

```bash
rails g model ranger name park:references
```

Run the db scripts
```bash
rails db:create
rails db:migrate
```

Rails should have generated this association in the model file:
#### app/models/ranger.rb
```ruby
belongs_to :park
```

Change the model file to associate Park with Ranger:
#### app/models/park.rb
```ruby
has_many :ranger
```

## Check your work on the command line
```bash
rails console
```
```ruby
yellowstone = Park.new(name: "yellowstone", description: "pretty cool")
yellowstone.save

ranger = Ranger.new(name: "roger", park: yellowstone)
ranger.save
```
Now we have a set of related records.
This active record query should work:
```ruby
Ranger.first.park.name
```

## Check your work in `psql`
```bash
rails dbconsole
```

```sql
SELECT * FROM rangers;
```

## Set up our requests:
### Nested route
```ruby
resources :parks do
  resources :rangers
end

resources :rangers
```

Test the routes they produce: `rake routes`

## Make the controllers:

*app/controllers/parks_controller.rb*
```ruby
class ParksController < ApplicationController

  def new
  end

  def create
    @park = Park.new(park_params)

    @park.save
    redirect_to @park
  end

  def show
    @park = Park.find(params[:id])
  end

private

  def park_params
    params.require(:park).permit(:name, :description)
  end

end
```


*app/controllers/rangers_controller.rb*
```ruby
class RangersController < ApplicationController

  def new
    @parks = Park.all
  end

  def create
    @ranger = Ranger.new(ranger_params)

    @ranger.save
    redirect_to @ranger
  end

  def show
    # deal with the case that we are trying to get a ranger for a park that doesn't exist

    @ranger = Ranger.find(params[:id])

    if params[:park_id].to_i != @ranger.park.id
      # do something
    end
  end

private

  def ranger_params
    params.require(:ranger).permit(:name, :park_id)
  end
end
```

## views
The app/views/parks/new.html.erb file looks the same:

*app/views/parks/new.html.erb*
```erb
<%= form_with scope: :park, url: parks_path, local: true do |form| %>

  <p>
    <%= form.label :name %><br>
    <%= form.text_field :name %>
  </p>

  <p>
    <%= form.label :description %><br>
    <%= form.text_area :description %>
  </p>

  <p>
    <%= form.submit %>
  </p>
<% end %>
```

## rangers#new

#### creating the ranger -> park association:

* Prepare in the `ranger#new`controller: make an instance variable with `@parks`
```ruby
@parks = Park.all
```
Create a pre-populated select tag:
```erb
<%= f.collection_select :park_id, @parks, :id, :name %>
```

All together it should look like this app/views/rangers/new.html.erb

*app/views/rangers/new.html.erb*
```erb
<%= form_with scope: :ranger, url: rangers_path, local: true do |form| %>

  <%= form.collection_select :park_id, @parks, :id, :name %>

  <p>
    <%= form.label :name %><br>
    <%= form.text_area :name %>
  </p>

  <p>
    <%= form.submit %>
  </p>
<% end %>
```
[http://api.rubyonrails.org/v5.1/classes/ActionView/Helpers/FormOptionsHelper.html#method-i-select](http://api.rubyonrails.org/v5.1/classes/ActionView/Helpers/FormOptionsHelper.html#method-i-select)

## logic for parks/:id/rangers

When you configure your routes like above:
```
resources :parks do
  resources :rangers
end
```

It creates routes that look like: (excerpt from `rake routes`)
```
      Prefix Verb   URI Pattern                         Controller#Action
park_rangers GET    /parks/:park_id/rangers(.:format)   rangers#index
     rangers GET    /rangers(.:format)                  rangers#index
```

Because both routes point to a single controller we need to write some logic to get this to work- rails doesn't do that for us.

```ruby
def index
  # test to see if we are at /parks/:id/rangers or /rangers
  if params.has_key?(:park_id)
    # get all the rangers for a specific park
    @rangers = Ranger.where(park_id: params[:park_id] )
  else
    # get all rangers
    @rangers = Ranger.all
  end
end
```


## Exercise:
Repeat the steps above.

#### Further:
Implement the display logic for the nested new route: `/parks/:id/rangers/new` and the nested show route `/parks/:id/rangers/:id`

#### Further:
Implement the display logic for the nested index route: /parks/:id/rangers

#### Further:
Implement edit for the nested route
