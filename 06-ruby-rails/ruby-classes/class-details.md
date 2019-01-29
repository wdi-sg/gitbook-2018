# More About Classes

## Scope of instance variables within a class

```
class Person

  def set_name( name )
    @name = name
  end

  def set_weight( weight )
    puts @name
    @weight = weight
  end
end

jane = Person.new
jane.set_name("jane")
jane.set_weight(50)
```

Class instance variables are accesible from anywhere within your class.

---

### Getters and Setters

Having created an *instance variable* in our object, we might want to *read* that *outside* our object. However, we have to define a method that will act as an **interface for reading** for this variable called a **Getter Method**.

```ruby
class Person

  def initialize(name)
    @name = name
  end

  def name
    @name
  end

end
```

Now we can *read* or *get*  the *name* outside the object.

```ruby
person = Person.new("Jane")
person.name

# => "Jane"
```

Similarly, we may need to *change* or *set* an instance variable from outside the object, so we create a method called a **setter method**.

```ruby
class Person

  def initialize(name)
    @name = name
  end

  def name
    @name
  end

  def name=(other)
    @name = other
  end
end
```

We can now *get* and *set* the name of a person using the  methods we created for `@name`.

```ruby
person = Person.new("Samantha")
person.name
# => "Samantha"

person.name = "Sam"
person.name
# => "Sam"
```

### Getters and Setters, the Ruby way

In ruby, the practice of creating a *getter* method is so common there is a shorthand that can be used at the top of a class definition called `attr_reader`.

```ruby
class Person

  attr_reader :name

  def initailize(name)
    @name = name
  end

  def name=(other)
    @name = other
  end
end
```


Similarly, we can do the same with the *setter* method using `attr_writer`

```ruby
class Person
  attr_reader :name
  attr_writer :name

  def initialize(name)
    @name = name
  end
end
```

Moreover, we have a shorthand for telling our class to create both a *getter* and a *setter* method called *attr_accessor*.

```ruby
class Person
  attr_accessor :name

  def initialize(name)
    @name = name
  end
end
```

### Private Methods

If we create a class `Person` with a name attribute and use `attr_accessor` to create the getters and setters as follows

```ruby
class Person
  attr_accessor :name
  attr_accessor :weight
  attr_accessor :location

  def initialize(name)
    @name = name
    @weight = '1kg'
    @location = 'SG'
  end
end
```

then anyone can read and access `Person#name`.

```ruby
person1 = Person.new("John")
person1.name

# => "John"
```

As part of creating a set of clean interfaces for our users (of this class) sometimes we want to make things inaccessible.

We can use the `private` keyword. Everything under the private keyword is private outside the class.

Example: We want to output a string for weight. Let's keep track of the number weight in a separate instance variable and convert it into the proper string depending on their location.

We don't need to expose these methods to the user of the class.

```ruby
class Person
  def initialize(name)
    @name = name
    @location = 'SG'
  end

  def weight
    if location == 'USA'
      return standard_weight
    end
    metric_weight
  end

  # some other code goes here to set the location

  private

  def standard_weight
    weight = @weight * 2.2
    "#{weight} lbs"
  end

  def metric_weight
    "#{@weight} kgs"
  end

end
```

We can also add private methods by defining new methods below the `private` keyword

```ruby
class Person

  def initialize(name)
    @name = name
  end

  private

  attr_accessor :name
  
  def make_call
    puts "Calling friends"
  end
end
```

Note that we can create a **public** method that calls a **private method**, because we are within the class.

```ruby
class Person

  def initialize(name)
    @name = name
  end

  def call
    make_call if name
  end

  private
  
  attr_accessor :name
  
  def make_call
    puts "Calling friends"
  end
end
```


#### Reference vs. Value
What is an **object** in ruby? Basically everything that isn't a keyword.

However, this can cause you some headaches if you're not careful.

Imagine we had the following

```ruby
arr1 = Array.new
arr1.push 1
arr1.push 2
arr2 = arr1
arr1 << 4
#=> [1,2]
arr2
#=> [1,2]
```

Wow, the second array completely changed. That's because `arr2` was a reference to `arr1`. Both variables represented the same **object**. The way around this is to copy the object.

```ruby
arr1 = Array.new
arr1.push 1
arr1.push 2

arr2 = Array.new(arr1)
arr1 << 3
#=> [1,2,3]
arr2
#=> [1,2]
```

This doesn't happen with single value variables:
```
my_num = 1

my_other_num = my_num

puts my_other_num

my_other_num += 2

puts my_other_num

puts my_num
```

Ruby and javscript need to be careful about the **size** of things- how much memory things take up. If they are not sure of the size of an array or object, the value they need to store the variable is a pointer/reference to a thing, rather than the thing itself ( a pointer to `arr1` vs. the value `1` itself)


### Chainable methods

What if I wanted to create a class that had **chainable** methods calling many methods in one line.

```ruby
class Person
  def initialize(name)
    @name = name
  end

  def greet
    puts "Hello I am #{@name}."
    puts "What is your name?"
    @other = gets.chomp
    puts "Nice to meet you, #{@other}."
  end

  def thank
    puts "Thank you for coming."
  end

  def farewell
    puts "Farewell, #{@other}"
  end
end
```


Trying to do:

```ruby
person1 = Person.new("john")
person1.greet.thank.farewell

#=> NoMethodError: undefined method `thank' for nil:NilClass
```

to achieve this we have to return a reference to the object after each method

```ruby
class Person
  def initialize(name)
    @name = name
  end

  def greet
    puts "Hello I am #{@name}."
    puts "What is your name?"
    @other = gets.chomp
    puts "Nice to meet you, #{@other}."
    self
  end

  def thank
    puts "Thank you for coming."
    self
  end

  def farewell
    puts "Farewell, #{@other}"
    self
  end
end
```

