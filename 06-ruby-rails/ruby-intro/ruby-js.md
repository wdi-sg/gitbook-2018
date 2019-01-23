## Ruby vs. Javascript

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

### Modules
Modules define a set of functionality and methods.
```ruby
Module Banana
  # code goes here
end
``

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

#### Keyword Arguments

You can specify default values for function arguments.

Works best when you have multiple arguments for your functions- argument order doesnt matter!
```
def say_hello(friend: 'Tim')
  puts "Hello, #{friend}!"
end

# note that there are no arguments
say_hello
```

#### Return Values
You don't need the return statement
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
