# 1 to Many Associations

## Objectives

* Implement one to many relationships through models in Rails
* Understand the model ordering opinion used by Rails
* Use the `collection_select` form helper to display a collection of associated items

## Create the app:
```
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
  * parks#new - add/remove rangers checkbox
  * parks#show
    * list all rangers with a specific park

  * rangers#new - add/remove rangers checkbox
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

```
rails g model park name description:text
```

```
rails g model ranger name park:references
```

## Check your work on the command line
```
yellowstone = Park.new(name: "yellowstone", description: "pretty cool")
ranger = Ranger.new(name: "roger", park: yellowstone)
```
Now we have a set of related records.
This active record query should work:
```
Ranger.first.park.name
```

## Make the controllers:
app/controllers/parks_controller.rb
```
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
```
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

    if params[:park_id].to_i != @ranger.park.id
      # do something
    end

    @ranger = Ranger.find(params[:id])
  end

private

  def ranger_params
    params.require(:ranger).permit(:name, :park_id)
  end
end
```

## Set up our requests:
### Nested route
```
resources :parks do
  resources :rangers
end

resources :rangers
```

## parks#new view helpers:

* Prepare in the controller: make an instance variable with `@parks`
```ruby
@parks = Park.all
```
```erb
<%= f.collection_select :park_id, @parks, :id, :name %>
```

[https://apidock.com/rails/ActionView/Helpers/FormOptionsHelper/collection_select](https://apidock.com/rails/ActionView/Helpers/FormOptionsHelper/collection_select)














