# Intro to Classes


## Objectives
* Understand difference between objects and classes
* Understand instance variables and instance methods
* Understand class variables and class methods


## Class Definition of a person

Let's create our first class.

**person.rb**

```ruby
class Person

end
```

This defines a **class** definition of a `Person`. The *class* keyword denotes the *begining* of a class definition.

To create a new *instance* of our *class* we write the following:

```ruby
Person.new
```

A class is an imprint of a thing we want to create.
![](https://media.giphy.com/media/6djJPJeaWwTrW/giphy.gif)



A particular instance of a *class* is a called an **object**. In general, languages that use *objects* as a primary means of *data abstraction* are said to be **Object Oriented Programming** (OOP) languages.


### Interfaces

With classes we are also creating interfaces (or methods / functions) that anyone using the class can call in order to do certain things.

Much like a library, when you create a set of methods within your class **you** are defining the way the person using that class will interact with it.

It doesn't matter __how__ the code in your class works, just that the interface gives methods to call and gives access to the relevant data.


We understand what this code would mean, even though we don't know how it would work:
```
person = Person.new("john")

person.birthday= "1/2/33"

puts person.calculate_age
```



### Objects


### Initialize and instance variables

In our class definition we can make use of an *initialize* method, which is run when a *new* instance of the class is created.

```ruby
class Person
  def initialize
    puts "A new person was created"
  end
end
```



We can also make use of **instance variables** that are defined for each particular object and are available throughout other *methods* in the object. These variables are prefixed by an *@* symbol, i.e. `@my_var`.

```ruby
class Person

  def initialize(name)
    @name = name
  end

  def greet
    puts "Hello! My name is #{@name}."
  end
end
```



Now, when we create a new *Person* we are required to specify the `name` of the person.

```ruby
person = Person.new("John")
person.greet

# => Hello! My name is John.
```

We can also use `initialize` to create a class instance with default values
```ruby
class Person

  def initialize
    @age = 0
  end

  def age
    puts @age
  end
end
```

Our first goal is to duplicate the functionality of a data structure that holds data, like a javascript object (or a ruby hash).

So:
```
const person = {
  name: "Susan Chan",
  weight: 123
};

console.log( person.name );  // get value out

person.name = "John"; // change a value
```

With our `Person` class, where we only hold one piece of data, the minimum code we need would look like this:

```ruby
class Person

  def initialize( age )
    @age = age
  end

  def get_age
    return @age
  end

  def set_age( new_age )
    @age = new_age
  end
end
```

### Using Classes
Now, we can create instances of a person, and they act just like any other kind of data.

```
person1 = Person.new( 22 )
person2 = Person.new( 12 )
person3 = Person.new( 52 )
person4 = Person.new( 42 )
person5 = Person.new( 32 )
```

Or:

```
people = [
  Person.new( 22 ),
  Person.new( 12 ),
  Person.new( 52 ),
  Person.new( 42 ),
  Person.new( 32 )
]
```

Or these class instances can be in a hash, or given as data and stored inside of other class instances.

```ruby
class Car
  def initialize( person )
    @owner = person
  end
end

person1 = Person.new( 32 )

car1 = Car.new( person1 )
```
### Pairing Exercise
(ruby for beginners)

