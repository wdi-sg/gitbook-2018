# 1 to Many Associations

## Objectives

* Implement one to many relationships through models in Rails
* Use the `collection_select` form helper to display a collection of associated items

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
  * parks#new - add/remove rangers select
  * parks#show
    * list all rangers with a specific park

  * rangers#new - add/remove rangers select
  * rangers#show

* Controllers
  * parks#new - get a list of all rangers here
  * parks#show
    * list all rangers with a specific park

  * rangers#new - get a list of all parks here
  * rangers#show

* Routes
  * We can nest the routes of one resource within the other one.

## Generating models

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

Change the model file to associate Park with Ranger:
#### app/models/park.rb
```ruby
has_many :ranger
```
#### app/models/ranger.rb
```ruby
has_one :park
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

## Make the controllers:
app/controllers/parks_controller.rb
```ruby
class ParksController < ApplicationController

  def new
    @rangers = Ranger.all
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


app/controllers/rangers_controller.rb
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

## Set up our requests:
### Nested route
```ruby
resources :parks do
  resources :rangers
end

resources :rangers
```

## views
The app/views/parks/new.hmtl.erb file looks the same
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

## rangers#new view helpers:

* Prepare in the controller: make an instance variable with `@parks`
```ruby
@parks = Park.all
```
Create a pre-populated select tag:
```erb
<%= f.collection_select :park_id, @parks, :id, :name %>
```

All together it should look like this app/views/rangers/new.hmtl.erb
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

## Exercise:
Repeat the steps above.

#### Further:
Implement the display logic for the nested new route: `/parks/:id/rangers/new` and the nested show route `/parks/:id/rangers/:id`

#### Further:
Implement the display logic for the nested index route: /parks/:id/rangers

#### Further:
Implement edit for the nested route
