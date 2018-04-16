## Validations

We can validate user input at several places in our rails app.

Strong params mean we can whitelist what we want to pass to the model.

The model has more fine-grained controls of what data it accepts.

[http://guides.rubyonrails.org/active_record_validations.html](http://guides.rubyonrails.org/active_record_validations.html)


### Strong Params
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

### Model validations
```
validates :title, presence: true, length: { minimum: 3, maximum: 20 }
```

```
validates :bio, length: { maximum: 500 }
```

```
validates :games_played, numericality: { only_integer: true }
```

```
validates :name, :login, :email, presence: true
```

```
validates :email, uniqueness: true
```

#### Output Model Validation Errors

Make a helper method in app/helpers/application_helper.rb
```
def show_errors(object, field_name)
  if object.errors.any?
    if !object.errors.messages[field_name].blank?
      object.errors.messages[field_name].join(", ")
    end
  end
end
```

Use the helper method for each form input
```
<p class="error">
  <%= show_errors(@user, :name) %>
</p>
```
