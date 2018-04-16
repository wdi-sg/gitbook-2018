# Many to Many Associations

## Objectives

* Implement many to many relationships through models in Rails
* Understand the model ordering opinion used by Rails
* Use the `collection_check_boxes` form helper to display a collection of associated items


## Create the app:
```bash
rails new parks -d postgresql
```

## What we need

* Models
  * Park
  * Ranger
  * ParksRangers (join table)
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
  * We can nest the routes of one resource within the other one.


## Generating models

Review of **Parks**

```bash
rails g model park name description:text
```

**Rangers**

```bash
rails g model ranger name
```

**ParksRangers**

```bash
rails g model parks_rangers park:references ranger:references --force-plural
```

# IMPORTANT
The join table with the two models **must** be plural and in alphabetical order if you want to follow the Rails convention. Also, `--force-plural` is needed so that the table will never be pluralized.

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

# ALSO IMPORTANT
When creating the M:M associations, the name of the model is pluralized when adding the `has_and_belongs_to_many` method. In ParksRangers, the associations will be singular and generated for you.

## Adding rangers on the command line:

```ruby
# assume the following:
some_park = Park.first
some_ranger = Ranger.first

# adding a ranger
some_park.rangers << some_ranger
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
resources :parks do
  resources :rangers
end

resources :rangers do
  resources :parks
end
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
<%= f.collection_check_boxes :park_ids, @parks, :id, :name %>
```


1. `:ranger_ids` refers to the model's rangers
2. `@rangers` refers to all the rangers available (pass from the controller `Ranger.all`)
3. `:id` refers to the value of the checkbox
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
