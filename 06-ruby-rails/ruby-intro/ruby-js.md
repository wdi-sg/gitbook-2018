# Ruby vs. Javascript

A guide to ruby through comparison with javascript.

### Comments

In JS, we use line and multiline comments.
```js
// here's a line comment

/* And a multiline
   comment
*/
```

In Ruby, multiline comments exist, but we generally use line comments with hashtags, for readability.
```ruby
# here is a line comment

# here is a block
# of comments
# for you to read
```
## Data Types

### Nothingness
Just as Javascript uses undefined or null, ruby uses `nil`

```ruby
my_bank_account = nil
```

### Booleans
A binary representation: either `true` or `false`

```ruby
is_operating = true
is_broken = false
```

### Numbers
Datatypes used to represent a number
* Fixnum: `23`
* Bignum: `23238923859348534535`
* Float: `23.23`

### Strings
A primative datatype used to represent a string of characters

#### Methods
```ruby
.split
.index
.upcase
.downcase
.sub
.gsub
.capitalize
```

#### Examples
```ruby
person = 'instructor'

person.split('')
#=> ["i", "n", "s", "t", "r", "u", "c", "t", "o", "r"]

person.index('tr')
#=> 3

person.upcase
#=> "INSTRUCTOR"

person.downcase
#=> "instructor"

person.sub('r', 'a')
#=> "instauctor"
# note that only the first character is replaced

person.gsub('r', 'a')
#=> "instauctoa"
# note that all character instances are replaced

person.capitalize
#=> "Instructor"

person.reverse
#=> "rotcurtsni"

person.length
#=> 10
```



## Conditionals

```ruby
if
elsif
else
unless
```
### No Falsy / Truthy
Ruby won't compare two things that are different types. You can cast them if you want to compare.


javascript:
```
"1" == 1 // -> true
"1" === 1 // -> false
```

ruby:
```
"1" == 1 // -> false
"1" == 1.to_s // -> true
```

#### If/Else
```ruby
course = "wdi"
if course == "uxdi"
  puts "Hello, User Experience Designer!"
elsif course == "fewd"
  puts "Hello, Front-End Developer"
elsif course == "wdi"
  puts "Hello, Immersed Student"
else
  puts "Who are you?"
end
```

## Data Structures

### Arrays
An indexed arrangement of objects

*several ways to create an array*

```ruby
arr = [1,2,3]
#=> [1,2,3]
```

#### Array Methods

```ruby
.sort
.reverse
.concat
.length
.push # (<<)
.shuffle
.shift
.unshift
.slice # (negative indicies or ranges, use ! to "splice")
.first
.last
.delete
.delete_at
.inspect
```

#### Examples

```ruby
numbers = [4, 2, 3, 1]

numbers.sort
# => [1, 2, 3, 4]

numbers.reverse
# => [1, 3, 2, 4]

numbers.concat([5, 6, 7])
# => [4, 2, 3, 1, 5, 6, 7]

numbers.length
# => 7

numbers.push(8)
# => [4, 2, 3, 1, 5, 6, 7, 8]

numbers << 9
# => [4, 2, 3, 1, 5, 6, 7, 8, 9]

numbers.shuffle
# => output will vary

numbers.shift
# => 4
numbers
# => [2, 3, 1, 5, 6, 7, 8, 9]

numbers.unshift(4)
# => [4, 2, 3, 1, 5, 6, 7, 8, 9]

numbers.slice(0, 2)
# => [4, 2]
numbers
# => [4, 2, 3, 1, 5, 6, 7, 8, 9]

numbers.slice!(0, 2)
# => [4, 2]
numbers
# => [3, 1, 5, 6, 7, 8, 9]

numbers.first
# => 3

numbers.last
# => 9

numbers << 'a'
# => [3, 1, 5, 6, 7, 8, 9, "a"]
numbers.delete('a')
# => "a"
numbers
# => [3, 1, 5, 6, 7, 8, 9]

numbers.delete_at(0)
# => 3
numbers
# => [1, 5, 6, 7, 8, 9]
```
### Hashes
A javascript object is called a hash in ruby. The word "object" refers to something else.

```ruby
dog = {
  :name => 'Hamlet',
  :breed => 'Pug',
  :fav_food => 'pate'
}
cat = {
  name: 'Simba',
  breed: 'American Shorthair',
  fav_food: 'Prosciutto'
}
dog[:name]
=> 'Hamlet'
cat[:fav_food]
=> 'Prosciutto'
```

### Symbols
Used in the place of a string, almost always as a key in a hash (object)

javascript:
```js
my_object = {
  foo: "bar"
}

my_hash["foo"] # -> "bar"
my_hash.foo # -> "bar"
```

ruby:
```ruby
my_hash = {
  foo: "bar"
}

my_symbol = :foo

my_hash[my_symbol] # -> "bar"
my_hash[:foo] # -> "bar"
```


### Enumerables and Collections
Enumberables are a kind of module that you use to manipluate **collections**

`.each` is a module method implemented for every kind of collection (array and hash)

```ruby
ark = ['cat', 'dog', 'pig', 'goat'] 
ark.each do |animal|
   puts animal
end
```

### do end blocks
These keywords denote the beggining and end of a block. `do end` is specifically used to specify the block for an enumberable

`def end` are used to specify a function

These are a kind of replacement for curly braces.

javascript:
```js
for( var item in my_object ){
  console.log( item );
}
```

ruby:
```ruby
my_hash.each do |item|
  puts item
end
```

### No Callbacks
You can't set anything to happen at a later time with anonymous functions.

## Functions

#### In Javascript

* anonymous: `function (param1, [..param2, [...]]){...}`,
* named: `function Name(param1, [..param2, [...]]){...}`
* uses lexical scope
* used as values (functional programming)
* require explicit return
* all `params` are optional

### In Ruby

* uses `def`
* does not capture scope
* not used as values
* implicitly returns last evaluation
* optional parameters must be specified

#### Examples
```ruby
def say_hello
  puts "Hello, World!"
end

say_hello()

# is the same as

def say_hello
  puts "Hello, World!"
end

# note missing parentheses
say_hello
```

In Ruby, leaving the `()` off of a function call is possible. Since functions can't be passed as values (i.e., aren't first-class), Ruby knows that we mean to call the function, so it calls it.

#### Parameters (Arguments)
```ruby
def say_hello(friend)
  puts "Hello, #{friend}!"
end

say_hello("Tim")

# is the same as
def say_hello(friend)
  puts "Hello, #{friend}!"
end

# note the lack of parentheses
say_hello "Tim"
```

#### Keyword Arguments
```
def say_hello(friend: 'Tim')
  puts "Hello, #{friend}!"
end

# note that there are no arguments
say_hello
```

#### Return Values
```ruby
def add(num1, num2)
  return num1 + num2
end

sum = add(2, 3)
puts "2 + 3 = #{sum}"

# is the same as
def add(num1, num2)
  # note the lack of explicit return
  num1 + num2
end

sum = add(2, 3)
puts "2 + 3 = #{sum}"
```

Ruby will automatically return the value of the last evaluated expression. This is called having "implicit returns". You are free to have an explicit return statement, but you don't have to.


### Scope
Ruby variable accessibility makes mch more sense than javascript's.


```ruby
foo = 'bar' # -> bar

if true == true
  puts foo # -> bar
  foo = 'baz'
  puts foo # -> baz

end

puts foo # -> baz

banana = ['a','b','c','d','e']

banana.each do |foo|
  puts "insuide each thingy: #{foo}"
  foo = 'ggg'
  puts "insuide each: #{foo}" # (array items)
  foo = 'haha'
end

puts foo # -> baz

def monkey
  puts foo # -> error, undefined
end

puts "monkey: #{monkey}"
```

### Tools

#### npm
In ruby, the functionality of `npm` is split into two different programs.

##### gem
This is the system to install all libraries for any ruby program.

gem is the online repository for all ruby libraries that are publicly published.

`gem install` is the command you run to install a library.

gem *does not* create a directory like `node_modules`. Your library files are stored in a central place on your computer.

By default, when you install a gem, that code is available anywhere on your computer.

##### Gemfile
`Gemfile` is the `package.json` of ruby.

##### bundler
In ruby you must manually add libraries you want in your projects to the `Gemfile`.

Installing those libraries on your system runs through a different command, one just for projects using a `Gemfile`.

```
bundle install
```

