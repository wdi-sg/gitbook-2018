# Many to Many Associations

## Objectives

* Implement many to many relationships through models in Rails
* Understand the model ordering opinion used by Rails
* Use the `collection_check_boxes` form helper to display a collection of associated items


## Create the app:
```bash
rails new parkng -d postgresql
```

## What we need

* Models
  * Park
  * Ranger
* Association
  * Park <-> ParksRangers <-> Ranger
  * Park `has_and_belongs_to_many` Rangers
  * Ranger `has_and_belongs_to_many` Parks
* Views
  * parks#new - add/remove rangers checkbox
  * parks#show
    * list all rangers with a specific park

  * rangers#new - add/remove rangers checkbox
  * rangers#show
    * list all parks with a specific ranger

* Controllers
  * parks#new - get a list of all rangers here
  * parks#show
    * list all rangers with a specific park

  * rangers#new - get a list of all parks here
  * rangers#show
    * list all parks with a specific ranger

* Routes

## Generating models / databse migrations

Review of **Parks**

```bash
rails generate migration parks
```

```
create_table :parks do |t|
  t.string :name
  t.text :description
  t.timestamps
end
```

touch app/models/park.rb (model filenames are singular)
```
class Park < ActiveRecord::Base
end
```


**Rangers**

```bash
rails generate migration rangers
```

```
create_table :rangers do |t|
  t.string :name
  t.timestamps
end
```


**ParksRangers**

```bash
rails generate migration parks_rangers
```

```
create_table :parks_rangers do |t|
  t.references :park
  t.references :ranger
  t.timestamps
end
```

# IMPORTANT
The join table with the two models **must** be plural and in alphabetical order if you want to follow the Rails convention. 

Note that if you want to name your join table something different, you can specify your own join model with `through:`

##Setting up associations

When you do `:references` it automatically creates the `belongs_to` relations on the join table, but we need to manually add the `has_and_belongs_to_many` to the ranger and park models.

**models/park.rb**

```ruby
has_and_belongs_to_many :rangers
```

**models/ranger.rb**

```ruby
has_and_belongs_to_many :parks
```

## Adding rangers on the command line:

Given:
```ruby
yellowstone = Park.new(name: "yellowstone", description: "pretty cool")
yellowstone.save

ranger = Ranger.new(name: "roger")
ranger.save
```


Add some stuff to the park:

```ruby
some_park = Park.first
some_ranger = Ranger.first

# adding a ranger
some_park.rangers << some_ranger
```

When creating a new park:
```ruby
yosemite = Park.new(name: "yosemite", description: "very noice", ranger_ids: [1,2])
```

Or assuming you already have 2 ranger instances:
```ruby
ecp = Park.new(name: "ecp", description: "so good", rangers: [roger, sam])
```

## Removing rangers on the command line:

```ruby
# assume the following:
some_park = Park.first
some_ranger = Ranger.first

# clear all of the park's rangers (leaves the rangers in the table)
some_park.rangers.clear

# removes a specific ranger from a park (leaves the ranger in the table)
some_park.rangers.delete(some_ranger)

# removes a specific ranger from a park (and deletes the ranger)
some_park.rangers.first.destroy
```

## Referencing and listing on the command line

Because Park and Ranger reference each other with `has_and_belongs_to_many` they can reference each other.

**Basic Examples**

```ruby
#lists all rangers
Ranger.all

#lists all parks
Park.all

#gets first park in the database
Park.first

#lists all rangers of first park
Park.first.rangers

#lists all parks of first ranger
Ranger.first.parks
```

## Set up our requests:

### Nested routes

```ruby

  root 'parks#index'
  get '/parks' => 'parks#index', as: 'parks'
  get '/parks/new' => 'parks#new', as: 'new_park'
  post '/parks' => 'parks#create'
  get '/parks/:id' => 'parks#show' , as: 'park'
  get '/parks/:id/edit' => 'parks#edit', as: 'edit_park'
  patch '/parks/:id' => 'parks#update'
  delete '/parks/:id' => 'parks#destroy'


  get '/rangers' => 'rangers#index', as: 'rangers'
  get '/rangers/new' => 'rangers#new', as: 'new_ranger'
  post '/rangers' => 'rangers#create'
  get '/rangers/:id' => 'rangers#show' , as: 'ranger'
  get '/rangers/:id/edit' => 'rangers#edit', as: 'edit_ranger'
  patch '/rangers/:id' => 'rangers#update'
  delete '/rangers/:id' => 'rangers#destroy'


  get '/parks/:park_id/rangers' => 'rangers#index', as: 'park_rangers'
  get '/parks/:park_id/rangers/new' => 'rangers#create', as: 'new_park_ranger'
  post '/parks/:park_id/rangers' => 'rangers#create'

  get '/rangers/:ranger_id/parks' => 'parks#index', as: 'ranger_parks'
  get '/rangers/:ranger_id/parks/new' => 'parks#create', as: 'new_ranger_parks'
  post '/rangers/:ranger_id/parks' => 'parks#create'
```

## view helpers:

* Prepare in the controller: make an instance variable with `@rangers`
```ruby
@rangers = Ranger.all
```
* Add checkbox to the form:
```erb
<%= f.collection_check_boxes :ranger_ids, @rangers, :id, :name %>
```

And to the new park form as well:
```ruby
@parks = Park.all
```
```erb
<%= f.collection_check_boxes :ranger_ids, @rangers, :id, :name %>
```


1. `:ranger_ids` refers to the name of the input and the key it will be in the request params
2. `@rangers` refers to all the rangers available (pass from the controller `Ranger.all`)
3. `:id` refers to the value of the checkbox when submitted in the request
4. `:name` refers to the label of the checkbox

## parks and rangers controllers:

Modify the `Park` controller to accept the `ranger_ids` and `parks_ids` array like so:

```ruby
def park_params
  params.require(:park).permit(:name, :description, :ranger_ids => [])
end
```
```ruby
def ranger_params
  params.require(:ranger).permit(:name, :park_ids => [])
end
```

## rangers#show

When showing a specific ranger, display the ranger name and the list of parks associated with it.
