## Install Rails
```
gem install rails
rails new blog -d postgresql
cd blog
```

## Start the Rails Server
```
createdb blog_development
bin/rails server
bin/rails generate controller Welcome index
```

## Say Hello
#### Open the `app/views/welcome/index.html.erb` file in your text editor. Delete all of the existing code in the file, and replace it with the following single line of code:
```html
<h1>Hello, Rails!</h1>
```

## Routes
#### Open the file `config/routes.rb` in your editor.

```ruby
Rails.application.routes.draw do
  get 'welcome/index'

  resources :articles

  root 'welcome#index'
end
```

## Test the Routes
```
bin/rails routes
```


#### Create an empty controller
```
bin/rails generate controller Articles
```


#### Open it and add the first method
```
class ArticlesController < ApplicationController
  def new
  end
end
```

#### Add this code into app/views/articles/new.html.erb
```
<%= form_with scope: :article, url: articles_path, local: true do |form| %>
  <p>
    <%= form.label :title %><br>
    <%= form.text_field :title %>
  </p>
 
  <p>
    <%= form.label :text %><br>
    <%= form.text_area :text %>
  </p>
 
  <p>
    <%= form.submit %>
  </p>
<% end %>
```


#### Add the create method in app/controllers/articles_controller.rb
```
class ArticlesController < ApplicationController
  def new
  end

  def create
  end
end
```

#### Try it out

#### Change it again:
```
def create
  render plain: params[:article].inspect
end
```

#### Create the model
```
bin/rails generate model Article title:string text:text
```

#### Look in the migration file

#### Run the migration
```
bin/rails db:migrate
```

#### Change the controller:
```
def create
  @article = Article.new(article_params)

  @article.save
  redirect_to @article
end

private
  def article_params
    params.require(:article).permit(:title, :text)
  end
```

#### Add show to the controller
```
def show
  @article = Article.find(params[:id])
end
```

#### Create a new file app/views/articles/show.html.erb with the following content:
```
<p>
  <strong>Title:</strong>
  <%= @article.title %>
</p>

<p>
  <strong>Text:</strong>
  <%= @article.text %>
</p>
```

## List all Articles


#### Add the index controller
```
def index
  @articles = Article.all
end
```


#### Add the view for this action, located at app/views/articles/index.html.erb
```
<h1>Listing articles</h1>
 
<table>
  <tr>
    <th>Title</th>
    <th>Text</th>
    <th></th>
  </tr>
 
  <% @articles.each do |article| %>
    <tr>
      <td><%= article.title %></td>
      <td><%= article.text %></td>
      <td><%= link_to 'Show', article_path(article) %></td>
    </tr>
  <% end %>
</table>
```

#### Open app/views/welcome/index.html.erb and modify it as follows:
```
<h1>Hello, Rails!</h1>
<%= link_to 'My Blog', controller: 'articles' %>
```

#### Add this "New Article" link to app/views/articles/index.html.erb placing it above the <table> tag:
```
<%= link_to 'New article', new_article_path %>
```

#### Add another link in app/views/articles/new.html.erb underneath the form, to go back to the index action:
```
<%= form_with scope: :article, url: articles_path, local: true do |form| %>
  ...
<% end %>

<%= link_to 'Back', articles_path %>
```

#### Add a link to the app/views/articles/show.html.erb template to go back to the index action as well, so that people who are viewing a single article can go back and view the whole list again:
```
<p>
  <strong>Title:</strong>
  <%= @article.title %>
</p>

<p>
  <strong>Text:</strong>
  <%= @article.text %>
</p>

<%= link_to 'Back', articles_path %>
```

### Add validation
```
class Article < ApplicationRecord
  validates :title, presence: true,
                    length: { minimum: 5 }
end
```


##### If @article.save fails in this situation, we need to show the form back to the user. To do this, change the new and create actions inside app/controllers/articles_controller.rb to these:
```
def new
  @article = Article.new
end

def create
  @article = Article.new(article_params)

  if @article.save
    redirect_to @article
  else
    render 'new'
  end
end
```

#### Modify app/views/articles/new.html.erb to check for error messages:
```
<%= form_with scope: :article, url: articles_path, local: true do |form| %>

  <% if @article.errors.any? %>
    <div id="error_explanation">
      <h2>
        <%= pluralize(@article.errors.count, "error") %> prohibited
        this article from being saved:
      </h2>
      <ul>
        <% @article.errors.full_messages.each do |msg| %>
          <li><%= msg %></li>
        <% end %>
      </ul>
    </div>
  <% end %>
```



### Editing

#### Add an edit action to the ArticlesController
```
def edit
  @article = Article.find(params[:id])
end
```


#### Create a file called app/views/articles/edit.html.erb
```
<h1>Edit article</h1>

<%= form_with(model: @article, local: true) do |form| %>

  <% if @article.errors.any? %>
    <div id="error_explanation">
      <h2>
        <%= pluralize(@article.errors.count, "error") %> prohibited
        this article from being saved:
      </h2>
      <ul>
        <% @article.errors.full_messages.each do |msg| %>
          <li><%= msg %></li>
        <% end %>
      </ul>
    </div>
  <% end %>

  <p>
    <%= form.label :title %><br>
    <%= form.text_field :title %>
  </p>

  <p>
    <%= form.label :text %><br>
    <%= form.text_area :text %>
  </p>

  <p>
    <%= form.submit %>
  </p>

<% end %>

<%= link_to 'Back', articles_path %>
```

#### Create the update action in app/controllers/articles_controller.rb
```
def update
  @article = Article.find(params[:id])

  if @article.update(article_params)
    redirect_to @article
  else
    render 'edit'
  end
end
```

#### Add the edit link to app/views/articles/index.html.erb to make it appear next to the "Show" link:
```
<td><%= link_to 'Edit', edit_article_path(article) %></td>
```


#### Also add one to the app/views/articles/show.html.erb template as well
```
<%= link_to 'Edit', edit_article_path(@article) %> |
```


## Deleting

#### Add the delete controller to app/controllers/articles_controller.rb
```
def destroy
  @article = Article.find(params[:id])
  @article.destroy

  redirect_to articles_path
end
```

Add a 'Destroy' link to your index action template (app/views/articles/index.html.erb)
```
<td><%= link_to 'Destroy', article_path(article),
        method: :delete,
        data: { confirm: 'Are you sure?' } %></td>
```
