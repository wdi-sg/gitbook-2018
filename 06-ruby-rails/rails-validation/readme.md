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

Validate a foreign key relationship:
```
validates :user, presence: true
```

Validates a many to many relationship:
```
class Course < ActiveRecord::Base

  has_and_belongs_to_many :teachers

  validate :has_one_teacher_at_least

  def has_one_teacher_at_least
    if teachers.empty?
      errors.add(:teachers, "need one teacher at least")
    end
  end
end
```
(see custom validations below)

#### Custom validations
```
class Invoice < ApplicationRecord
  validate :active_customer, on: :create

  def active_customer
    errors.add(:customer_id, "is not active") unless customer.active?
  end
end
```
From [here](http://guides.rubyonrails.org/active_record_validations.html#custom-methods)

#### Controllers for implementing validations errors:
```
if @ranger.save
  redirect '/'
else
  @parks = Park.all
  render 'new'
end
```

#### Output Model Validation Errors

Make a helper method in app/helpers/application_helper.rb
```
def show_errors(object, field_name)
  if object && object.errors.any?
    if !object.errors.messages[field_name].blank?
      object.errors.messages[field_name].join(", ")
    end
  end
end
```

Use the helper method for *each* form input
```
<p class="error">
  <%= show_errors(@user, :name) %>
</p>
```

Create custom error messages: [https://stackoverflow.com/questions/808547/fully-custom-validation-error-message-with-rails](https://stackoverflow.com/questions/808547/fully-custom-validation-error-message-with-rails)


### pairing exercise
Select one of your rails app.

Apply one random validation from above to one of your models.

Check to see if the validation works.

Implement the error message output.

#### further
Try out the other kinds of validations.

Complete the error output for each input in your form.

#### further
Implement a custom validation.

