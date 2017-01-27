#AJAX in Rails

In Rails there are three main ways we can work with AJAX.
1. We can expose API end points that return json data, then in client-side JS use the data to update the DOM.
2. We can use .js.erb files to construct templates views that can be appended to the page automatically for us (also using jQuery behind the scenes).
3. We can have forms and links submit data using a 'remote' option, we can then listen for custom Rails events in our client-side JS.

## 1. Plain Old Ajax

AJAX calls in Rails can be done if you have a controller action that returns JSON data. Here's an example. Let's assume this controller action can be accessed via the route `GET /names`.

```ruby
class NamesController < ApplicationController
  def names
    names = Name.all
    render json: names
  end
end
```

When accessing `GET /names`, the response will contain JSON data.

In order to access this route via AJAX, we can create a JavaScript file that accesses this route. We'll need to account for Turbolinks on the page by encapsulating the AJAX call inside an event called `ready page:load`.

```js
$(document).on('ready page:load', function() {
  $.get('/names').done(function(data) {
    console.log(data);
  });
});
```

This AJAX call can be triggered on form submission or button click by encapsulating the call within an event listener function. Example:

```js
$(document).on('ready page:load', function() {
  $('#button').click(function() {
    $.get('/names').done(function(data) {
      console.log(data);
    });
  });
});
```

## 2. JS EJS Views

Rather than rendering html files we can get rails to render js files, that contain the necessary html to inject on the page.

For example, in our index.html.erb page we may have a link, that when cliced we want to display a form.

**index.html.erb**

```erb
<%= link_to 'Show Form', new_thing_path, remote: true %>
```
When clicked rails will send an AJAX request to our new_thing_path which we can then tell to render our form (which will default to looking a form.js.erb file)

**things_controller.rb**

```ruby
# GET /things/new
def new
  @thing = Thing.new
  render :form
end
```

Inside that file, we can then execute some JavaScript to render a html partial in a particular element on the page. For example lets render the form_template in the form_container.


**form.js.erb**

```erb
$("#form_container").html("<%= escape_javascript(render partial: 'form_template', locals: { thing: @thing } ) %>");

```

**_form_template.html.erb**

```erb
<%= form_for(thing, remote: true) do |f| %>
  ...

  <%= f.label :name %>
  <%= f.text_field :name, required: true %>

  <%= f.submit %>
<% end %>
```

As this form is also using the remote option, we will be able to render a create.js.erb file to append the created object to the page.

Check out [Things in this more comprehensive example](https://github.com/wdi-sg/rails-ajax-examples)

## 3. Remote Forms/Links & JS Events

Similar to the 2nd example, we can use the remote option on forms and links, so Rails will send AJAX requests. For example we may have a form on our index.html page.

**index.html.erb**

```erb
<%= form_for(:stuff, remote: true) do |f| %>
  ...

  <%= f.label :name %>
  <%= f.text_field :name, required: true %>

  <%= f.submit %>
<% end %>
```

Similar to the 1st example, inside out controller, we can return JSON data.

**stuff_controller.rb**

```ruby
# POST /stuff
def create
  @stuff = Thing.new(stuff_params)
    if @stuff.save
      render json: @stuff
    else
      render json: { :errors => @stuff.errors.full_messages }, :status => :unprocessable_entity
    end
end
```
We can then listen for the custom AJAX result events in our client-side js files. For example...

**assets/javascripts/stuff.js**

```js
$('#add-stuff').on('ajax:success', function () {
    alert('New Stuff Created!')
})
```

The following events are supported:
- ajax:before      // fires before the request starts, provided a URL was provided in href or action
- ajax:loading     // fires after the AJAX request is created, but before it's sent
- ajax:beforeSend  // equivalent to ajax:loading in earlier versions
- ajax:error       // equivalent to ajax:failure in earlier versions
- ajax:success     // fires after a response is received, provided the response was a 200 or 300 HTTP status code
- ajax:failure     // fires after a response is received, if the response has a 400 or 500 HTTP status code
- ajax:complete    // fires after ajax:success or ajax:failure
- ajax:after       // fires after the request events are finished

Check out [Stuff in this more comprehensive example](https://github.com/wdi-sg/rails-ajax-examples)
