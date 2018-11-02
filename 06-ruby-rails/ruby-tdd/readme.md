# TDD

## What is TDD - Test Driven Development
TDD is an extension of the agile philosphy, which also includes a way to ensure that your application will run the way you think it will.

The way that this is accomplished is with *software tests*. A set of code that runs verification tests against the application you write.

TDD specifically is a process that builds your app and writes those tests where *the test is written before you write your app code*.

Practitioners of TDD feel that the process helps them to think about the featrues of their app in a more holistic way, and that it prevents bugs.

### Learning Objectives
- Understand the RSPEC testing library
- Pass a simple set of ruby class tests
- Understand the TDD process
- Write a new class within the TDD process
- Understand the rails TDD setup

### RSPEC Testing library
The main tool we will be using is the RSPEC testing library. This is the tool that runs the verifications (tests) we write.

A simple example looks like this:
```
it "adds 2 and 2 together" do
  expect(2 + 2).to eq(4)
end
```

This example is a basic demonstration of what a test does. It simply verifies the output of some piece of code.

We will be using this to test that our code does what we think it should do.


### RSPEC Setup
Gemfile
```ruby
source "https://rubygems.org"

gem 'rspec'
```

Install it
```bash
bundle install
```

Create a class file:
```bash
touch calculator.rb
```

```ruby
class Calculator
  attr_accessor :sum
end
```

Create the testing files:
```bash
rspec --init
```
(checkout what's inside spec/spec_helper.rb)

```
touch spec/calculator_spec.rb
```

```
require 'calculator'

RSpec.describe Calculator, "#score" do
  it "sums the pin count" do
    calc = Calculator.new
    calc.sum = 2
    expect(calc.sum).to eq(2)
  end
end
```

Run these files:
```
rspec
```

### Testing with RSPEC
The point of testing is simply to make sure that some representative test cases work properly, and as you continue to code, that they still work.

```
calc.add(2,2)
expect(calc.sum).to eq(4)
```

- you don't need to cover *evey single possible case*, only some representative cases and most logical cases - e.g., what if the numbers are negative, what if it's zero, what if it's very large
- test that errors are well handled - does your entire program crash when given a certain input?
- what are the main funcionalities of your class? How do you run them and what is the output? It could help someone to understand your code to look at what tests you write.

### TDD Process
We have been talking about tests that simply verify the behavior of your code. TDD is the process for writing code that begins with a test, reversing the process we have been talking about.

TDD is closely linked with agile in the sense that it is a well defined process that has distinct stages of progression for your code.

The base unit of TDD is a feature- this means one complete unit of functionality (not neccessarily a feature that the user uses). This could be given to you by the scrum master / product manager or come out of a user story, but generally comes out of some user requirement. This means that *you know what the behavior of the feature is* before you begin work on the code.

For example, I already know that my `Calculator` class will be able to add numbers together before I decide exactly how to implement that. I understand from a high level what the inputs need to be and what the  outputs need to be.

When I begin the TDD process I will be writing a test that exactly specifies a representative input and output.

Only after I am done writing the test to I begin writing my code.

### Exercise
[https://github.com/wdi-sg/rspecalculator](https://github.com/wdi-sg/rspecalculator)


### TDD / Testing in Rails
All of these examples we've seen have been for ruby classes. When you are a back-end developer it can be fairly easy to write code that matches up with your tests.

In a lot of other situations though, testing can be a difficult proposition. What if your class relies on an outside API (twitter, facebook, etc.) - what if it relies on many other classes? How do you test when something takes too long?

For all of these scenarios there are testing strategies, but it's quite rare for tests to cover all of these possible cases within an app. If you did this you would spend more time working on your tests than working on your app.

Most tests are a compromise of what is testable and what is deemed "safe" or non-mission critial code. You will spend more time writing careful tests for the core of an app.

Some of the libraries you are likely to encounter in a rails app are:
- factory bot [https://github.com/thoughtbot/factory_bot](https://github.com/thoughtbot/factory_bot)
- this library makes sure that other dependant classes are "stubbed" or work the way we want them to.
- vcr [https://github.com/vcr/vcr](https://github.com/vcr/vcr)
- this library records your api interactions so that you can use them to test your code later.


#### End-to-end Testing
One of the high-level strategies for testing an app is called end-to-end, or acceptance testing. Here, we try to actually run the app as a user, using simulated actions in the browser.

The library that is used to write these browser tests is [Capybara](https://github.com/teamcapybara/capybara).

End-to-end testing is considered the last line of defense against bugs.

When TDD is done in the context of rails it usually begins with a user story and an end-to-end test.

Example:
```
As a farmer I want to be able to calculate my current total harvest so that I can plan my day.
```
Or
```
As a user I want to be able to sign in so that I can see by bank balance.
```

You would then write a test that tests this set of functionality.
```
it "signs me in" do

  visit '/sessions/new'

  within("#session") do
    fill_in 'Email', with: 'user@example.com'
    fill_in 'Password', with: 'password'
  end

  click_button 'Sign in'
  expect(page).to have_content 'Success'
end

```
Run the test, and when it fails begin writing the functionality that will allow the test to pass.

### Exercise
Setup capybara in rails:
From: [https://github.com/teamcapybara/capybara](https://github.com/teamcapybara/capybara)

Your Gemfile:
```
group :development, :test do
  gem 'rspec-rails', '~> 3.7'
end
```

Add development group to capybara gemfile stuff
```
group :development, :test do
  # Adds support for Capybara system testing and selenium driver
  gem 'capybara', '>= 2.15', '< 4.0'
  gem 'selenium-webdriver'
  # Easy installation and use of chromedriver to run system tests with Chrome
  gem 'chromedriver-helper'
end
```

```
bundle install
```

Generate the default rspec rails files:
```
rails generate rspec:install
```

Add capybara inside of `spec/rails_helper.rb`
```
require 'rspec/rails'
require 'capybara/rspec'
Capybara.default_driver = :selenium_chrome
```

Check that it works:
```
bundle exec rspec
```

Create your first feature spec:
```
mkdir spec/features
touch spec/features/homepage_spec.rb
```

```
require 'rails_helper'

describe "the homepage", type: :feature do
  it "renders homepage" do
    visit '/'
  end
end
```

Run it:
```
bundle exec rspec
```

This test should pass- the browser should be able to visit the root route.

In a TDD workflow let's add a test we know will fail.
Add this after `visit`:
```
expect(page).to have_content 'Success'
```

Run the test again.

Write the code that will pass the test.
