# Exceptions

Execptions are a way to end the current execution of your program. They are basically a different kind of control flow.

More specifically you can use them to "skip up" several layers of your application and handle errors in a centralized place.

This is an important step in creating a more "professional" kind of app that gracefully handles all possible error states.

Basic exception:
```ruby
begin
  puts 'I am before the raise.'
  raise 'An error has occured.'
  puts 'I am after the raise.'
rescue
  puts 'I am rescued.'
end
```

Exception with built in error classes:
```ruby
begin
  File.open('p014constructs.rb', 'r') do |f1|
    while line = f1.gets
      puts line
    end
  end

  # Create a new file and write to it
  File.open('test.rb', 'w') do |f2|
    # use "" for two lines of text
    f2.puts "Created by Satish\nThank God!"
  end
rescue StandardError => msg
  # display the system generated error message
  puts msg
end
```

Exception with custom error classes:
```ruby
  # When someone tries to set a last name, enforce rules about it.
  def last=(last)
    if last == nil or last.size == 0
      raise ArgumentError.new('Everyone must have a last name.')
    end
    @last = last
  end
end
```

The class itself:
```ruby
class ArgumentError < StandardError
    def initialize(msg="Argument Banana!")
        super
    end
end
```

Rescue a custom exception:
```ruby
begin
  User.last
rescue ArgumentError => msg
  # display the system generated error message
  puts msg
end
```

Warning: you are able to rescue any exception:
```ruby
begin
  # stuff
rescue Exception
  # generic exception rescue
end
```

[But DON'T DO IT.](https://robots.thoughtbot.com/rescue-standarderror-not-exception)
