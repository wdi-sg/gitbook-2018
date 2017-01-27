# Layouts, Partials, and Views

### Objectives
- Describe how templates & views work together
- Use partials for static content and rendering dynamic content

## Using views with Rails

In Rails, the logic for the rendering a view is quite straightforward. Given that every route in Rails will execute a method inside a controller, when the method is executed, Rails will look for:

1. A folder inside `views` corresponding to the controller's name (folder `posts` for `PostsController`).
2. A file with the method's name and `.html.erb`.

For example , if we call `http://localhost:3000/posts`, Rails will execute the method `index` in the controller `posts` and then look for a template located in `app/views/posts/index.html.erb`  This works when the method always renders the same template.

In some cases though, you may want to render a template with a different name than the current method. Let's take a look at this action:

```ruby
def create
  @post = Post.new(post_params)

  respond_to do |format|
    if @post.save
      format.html { redirect_to @post, notice: 'Post was successfully created.' }
      format.json { render :show, status: :created, location: @post }
    else
      format.html { render :new }
      format.json { render json: @post.errors, status: :unprocessable_entity }
    end
  end
end
```

Based on the result of `@post.save`, the method will execute either the code in the `if` or in the `else`.  The code `format.html` or `format.json` means that Rails will understand the format asked by the user, html or JSON.  We will look at JSON later in the course, so for the moment, we will only look at the lines starting with `format.html`.

In the `if` case , we can see that in the code executed in the block `redirect_to @post, notice: 'Post was successfully created.'` This code will redirect the request to the show method. `redirect_to` the `@post` object in rails means "go to the method to show only this object".

In the `else` case, the code executed is `render :new` - this means, Rails will show the template `app/views/post/new.html.erb`.  This template uses an instance variable `@post`. In this case, it will use the instance variable defined at the start of the create method.

To sum it up, Rails will, by default, render the template that has the name of the current method in the controller, unless there is a `render` statement in the method that tells Rails to use a different template.

There are different syntaxes for render, and they all do the same action described above but the rule of thumb is to use the simplest one that makes sense for the code you are writing.

```ruby
render :edit
render action: :edit
render "edit"
render "edit.html.erb"
render action: "edit"
render action: "edit.html.erb"
render "books/edit"
render "books/edit.html.erb"
render template: "books/edit"
render template: "books/edit.html.erb"
render "/path/to/rails/app/views/books/edit"
render "/path/to/rails/app/views/books/edit.html.erb"
render file: "/path/to/rails/app/views/books/edit"
render file: "/path/to/rails/app/views/books/edit.html.erb"
```

## Using Layouts

By default, Rails will render the layout `application.html.erb` but sometimes, you want to render a template in a different layout.

For instance, to create a layout called `sidebar.html.erb`

```bash
touch app/views/layouts/sidebar.html.erb
```

Take this template and add it into the `sidebar.html.erb`:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Sidebar Template</title>
</head>
<body>
  <header>
    <h1>My Website</h1>  
    <ul>
      <li>Menu 1</li>
      <li>Menu 2</li>
      <li>Menu 3</li>
      <li>Menu 4</li>
    </ul>
  </header>
  <main>
    <%= yield %>
  </main>
  <footer>
    <ul>
      <li>About us</li>
      <li>Team</li>
      <li>Terms and conditions</li>
    </ul>
  </footer>
</body>
</html>
```

This will help us to differentiate the layouts.

In the controller method `index`, add this to the end of the method:

```ruby
render layout: "sidebar"
```

This line will just tell Rails to use the same logic of template rendering, but instead of using the default `application.html.erb`, it will render the template inside `sidebar.html.erb`. You can also specify the layout for a whole controller by specifying at the top of you Controller class, before your actions.

```ruby
layout: "sidebar"
```

#### Using partials

A best practice is to always keep every template as small as possible. A rule of thumb would be to keep shorter than 50 lines, for the sake of readability.

So for example, if your website has a layout with a top menu, the content and then the footer, this should all be in the layout, as it is rendered on every page.  But if all this html is in one file, the layout will end up being 200+ lines of code.

When this happens, you should think about splitting the template into partials. Partials are just chunks of a template that will be rendered into another one, like children of the layout.

This layout in `sidebar.html.erb` will soon be too long and unreadable, thus we need to split into different parts:

Create a new file:

```bash
mkdir app/views/application
touch app/views/application/_header.html.erb
```

And inside move the following from `sidebar.html.erb`:

```erb
<header>
  <h1>My Website</h1>  
  <ul>
    <li>Menu 1</li>
    <li>Menu 2</li>
    <li>Menu 3</li>
    <li>Menu 4</li>
  </ul>
</header>
```

Now let's create another file:

```bash
touch app/views/application/_footer.html.erb
```

And inside move the following from `sidebar.html.erb`:

```erb
<footer>
  <ul>
    <li>About us</li>
    <li>Team</li>
    <li>Terms and conditions</li>
  </ul>
</footer>
```

Now the main `sidebar.html.erb` file is back to a normal size, we just need to include the partials and the final result will then be the same than before.

**Note:** Every partial needs to have an underscore as the first character - this way Rails knows that it is not a proper template but only a partial that will be included in a template.

Let's now call the partials in the layout:

```erb
<!DOCTYPE html>
<html>
<head>
  <title>Sidebar Template</title>
</head>
<body>
  <%= render "header" %>
  <main>
    <%= yield %>
  </main>
  <%= render "footer" %>
</body>
</html>
```

Rails will automatically look in the folder `app/views/application/` for a file that is called by the name given to the method `render` with an underscore prefix.
