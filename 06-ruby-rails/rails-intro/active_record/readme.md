# Active Record

<span class="non-slide"></span><span class="non-slide"></span>
<span class="non-slide"></span><span class="non-slide"></span>

## Screen Casts for code
- [Part 1](https://youtu.be/wbAPxnuIXck)
- [Part 2](https://youtu.be/J9w1CDXrOXc)
- [Part 3](https://youtu.be/NQNBnmpG5tE)
- [Part 4](https://youtu.be/EkD5LEyALvM)
- [Part 5](https://youtu.be/VOH26fAzc4o)
---

## Learning Objectives

- Define the term "ORM" and why we use it over a database language
- Explain what Active Record is and what problems it solves
- Explain "convention over configuration" and how it relates to Active Record
- Define a class that inherits from Active Record
- Utilize Active Record to perform CRUD actions on a database
- Differentiate between class and instance methods in Active Record classes/objects

---

## Framing (5 min / 0:05)

So far, we've learned principles of object-oriented programming and how to get data to persist in a database using SQL. However, we now need some way to connect the two. We need to be able to retrieve data from a SQL database and store it in Ruby objects that we can use in our application. While we could use the `pg` gem to write and execute SQL commands in our Ruby code, this process is onerous and can result in a lot of repetitive code. We would have to write very long SQL statements to do even simple CRUD actions.

It'd be **really** nice if a bunch of genius programmers had already worked out some kind of way to interface between the database and our servers/applications in order to streamline the process of reading and writing data to and from a database. Enter **ORMs** and [The Active Record pattern](https://en.wikipedia.org/wiki/Active_record_pattern).

---

## ORM's & Active Record (10 min / 0:20)

- Object Relational Mapping: A programming technique for converting data between incompatible type systems in object-oriented programming languages - from [Wikipedia](https://en.wikipedia.org/wiki/Object-relational_mapping)

We need a way to encapsulate the data from our databases into objects so that we can use them in our server applications. ORM's serve that purpose. Remember those tables we created in SQL? Well, now those rows of data are objects (instances of classes) on our server now. That's what ORM's do.

---

![ORM-ActiveRecord](https://github.com/wdi-sg/activerecord-intro/blob/master/orm.jpg?raw=true)

---

More concretely ORM's:

- *'Map'* (translate) objects to rows in our DB (and vice versa)
- **Conventions**:
  - 1 table per Model/Class/Entity
  - Model names are singular and capitalized, table names are lowercase and plural
  - each column represents an attribute for that model
- Table associations are handled using foreign keys

It just so happens you will be learning one of the best ORM's on the market. It has some of the best documentation and best syntax (because Ruby is awesome). This ORM is Active Record.

<span class="non-slide"></span><span class="non-slide"></span>
<span class="non-slide"></span><span class="non-slide"></span>

> Active Record is the M in MVC - the model - which is the layer of the system responsible for representing business data and logic. Active Record facilitates the creation and use of business objects whose data requires persistent storage to a database. It is an implementation of the Active Record pattern which itself is a description of an Object Relational Mapping system. (from AR docs)

---

## Active Record

Active Record is an ORM (packaged in a Ruby gem) that allows us to translate database records into objects that we can use in our Ruby applications.

<span class="non-slide"></span><span class="non-slide"></span>
<span class="non-slide"></span><span class="non-slide"></span>

In order to use Active Record in our Ruby code to manipulate data in a database, we need to be able to talk about the **models** of our data.

But before we even do that, we have to decide on what our data is! How much of the real world are we going to attempt to represent in our programs? What data do our programs need to fulfill their purpose? To be able to think about how to model our data, we need a process where we can precisely identify what we need represented in our programs as data.

Programmers are constantly modeling domains, real world, fictional, abstract, mathematical or otherwise.

<br>
<details>

<summary>What is Domain modeling and why do we do it?</summary>

> Domain modeling is the act of describing entities and their relationships in an application's data. This method is useful for deciding data what needs to be persisted and how it should be organized.

</details>
<br>

---

When we model data, we tend to be talking about the **Nouns** in our application. These are the names of the *tables* in our database and the names of our Ruby *classes*.

Likewise when we write queries, we use **Verbs** to describe the specific data we want.
---

Essentially, in order to store and retrieve information, a lot of what we do today in Ruby will look like some form of the equation:


> **Noun** + **Verb** = **Data**
>
> Class.method # Class (Noun) method (Verb)
>
> Taco.all # returned all Taco data!
>
> 🌮 🌮 🌮 🌮 🌮 🌮 🌮 🌮 🌮 🌮 🌮 🌮


With the help of Active Record, we can begin to write programs that follow this simple pattern to manipulate data.

---


### Convention Over Configuration (10 min / 0:30)

Before we get started with code, let's highlight a reoccurring theme with Active Record, Rails, and frameworks in general. You'll often hear us say, "Convention Over Configuration."

<span class="non-slide"></span><span class="non-slide"></span>
<span class="non-slide"></span><span class="non-slide"></span>
<span class="non-slide"></span><span class="non-slide"></span>




Before we discuss the concept as a class, take 30 seconds to think about what that phrase means--why *might* we prefer convention over configuration?

<details>
<summary><strong>Question:</strong>  Without getting into the specifics of AR, what do you think we mean by convention over configuration?</summary>
<br>

---

> Active Record, Rails, and other frameworks have a whole bunch of conventions that they follow so that you do not have to mess with different configuration details later. These conventions exist because developers arrive at a consensus on best practices. These road-tested conventions allow us to spend less time trying to configure when there already is an accepted way to do things. Thanks to the programmers who have come before us, we inherit a well-designed, default configuration that spares us from many headaches that we'd encounter if we were building things from scratch (yikes!).

---
>
> Some of the common ones we will encounter are naming conventions such as:
  - plural vs single
  - Capitalized
  - ALL_CAPS_SNAKE_CASE
  - lowercase
  - camelCase
  - kabob-case
  - snake_case.

  Obeying the naming conventions in Active Record, particularly regarding what is singular vs. what is plural, saves you a good deal of headaches.

</details>

---

#### If you don't follow the conventions, you're going to have a bad time.

Obeying the naming conventions in Active Record saves you a good deal of headaches.

---

# Coding Time!

---

### Gemfiles
To install ruby library packages, we use `gem`. The `gem` command is equivalent to `npm install`.

Gemfiles are equivalent to `package.json`. You'll notice that we will also have a Gemfile.lock just like we had a `yarn.lock` file.

To install gems using a file we use the `bundle install` command. This is equivalent to `yarn add`.

Your gemfile is simply named `Gemfile`. Uppercase, no file extension.

It should contain:
```ruby
source "https://rubygems.org"

gem "pg"  # this gem allows ruby to talk to postgres
gem "activerecord"  # this gem provides a connection between your ruby classes to relational database tables
gem "byebug"  # this gem allows access to REPL
```

Install all the gems specified in the gem file with the command:
```
bundle install
```

---
#### Defining our Models

In the `artist.rb` file, we define our `artist` model:

```ruby
class Artist < ActiveRecord::Base
  # AR classes are singular and capitalized by convention
end
```

> In this Ruby file, we create a class of Artist that inherits from `ActiveRecord::Base`. Essentially, when we inherit from `ActiveRecord::Base`, it gives this class a whole bunch of functionality that maps the Ruby `Artist` class to the `artists` table in Postgres.

---


## Inheritance
We are using this stntax for ruby inheritance:
```
class Artist < ActiveRecord::Base
```

This means we can use Artist class in much the same way we would use an ActiveRecord class. Except that we can add in all our Artist class stuff. The code looks clean and isn't cluttered by active record code, and we don't care how the active record methods we get for free *actually* work.

If you want a bit more detail on inheritance in ruby check the [gitbook.](../../ruby-inheritance/readme.html)

---


### app.rb
Let's setup the main file.

We need to use require a bunch of times to gather together all the libs.

---
```
require "bundler/setup" # require all the gems we'll be using for this app from the Gemfile. Obviates the need for `bundle exec`
require "pg" # postgres db library
require "active_record" # the ORM
require "byebug" # for debugging


# initialize a connection to the database
ActiveRecord::Base.establish_connection(
  :adapter => "postgresql",
  :database => "tunr_db"
)

require_relative "artist" # require the Artist class definition that we defined in the artist.rb file

byebug

puts "end of application"
```
<span class="non-slide"></span><span class="non-slide"></span>
<span class="non-slide"></span><span class="non-slide"></span>


<details>
<summary>Did you get an error from <code>bundle install</code>?</summary>
If you get an error message like this one:

```
An error occurred while installing json (1.8.3), and Bundler cannot continue.
Make sure that `gem install json -v '1.8.3'` succeeds before bundling.
```

Then run this command: `$ bundle update`
</details>

---


### Setting up the db

---

### schema.sql
In rails `tables.sql` is called `schema.sql`

(We will see tomorrow that there is more to the rails system of db manegement)

---
```sql
DROP TABLE IF EXISTS songs;
DROP TABLE IF EXISTS artists;

CREATE TABLE artists(
  id SERIAL PRIMARY KEY,
  name TEXT,
  photo_url TEXT,
  nationality TEXT
);

CREATE TABLE songs(
  id SERIAL PRIMARY KEY,
  title TEXT,
  album TEXT,
  preview_url TEXT,
  artist_id INT
);
```

---

### seeds.sql
Is the same as it was before. It contains sql for each row.
(we will be getting rid of this format for seed soon)

---

### running the db configs:

Continue with the following `bash` commands:

```bash
createdb tunr_db
psql -d tunr_db < schema.sql
psql -d tunr_db < seeds.sql
```

Check you did everything correctly.

---

## Using Active Record

- Run your program(`$ ruby app.rb`)
- When you see the `byebug` REPL, run this ruby code: `Artist.first`

Your output should be something like this (it won't be the same letters and numbers next to `#<Artist:`)
```bash
pry(main)> pry(main)> Artist.first
=> #<Artist:0x007ff851821bc0
 id: 1,
 name: "Weird Al Yankovich",
 photo_url: "http://i.huffpost.com/gen/1952378/images/o-WEIRD-AL-facebook.jpg",
 nationality: "American">
```

---

Great! We've got everything done that we need to get setup with single model CRUD in our application. Let's run it in the terminal:

```bash
$ ruby app.rb
```

If we want to see the output of the active record class as it works, we can drop this line into our `app.rb` file:
```
ActiveRecord::Base.logger = Logger.new(STDOUT)
```

---

When we run this app, we can see that it drops us into byebug. Let's write some code in byebug to update our database... **IN REALTIME!!!**

We'll create an instance of the `Artist` object on the Ruby side:

> **Note** the syntax for creating a new instance.

```ruby
kanye = Artist.new(name: "Kanye West", nationality: "American")
```

To save our instance to the database we use `.save`:

```ruby
kanye.save
```

---

If we want to initialize an instance of an object AND save it to the database we use `.create`:

```ruby
elvis = Artist.create(name: "Elvis Presley", nationality: "American")
```

<!-- SQL for create  -->
<details>
<summary>This would roughly translate to `SQL` code</summary>

<code>
INSERT INTO artists (name, nationality) VALUES ('Elvis Presley', 'American');
</code>

</details>

<br>

**Question:** Why is there a distinction between when it's saved in one command versus two?

One really handy feature we get from an Active Record inherited class is that all of the attribute columns of our model have `attr_accessor`'s as well. So we can do things like:

```ruby
kanye.name
# gets the name of kanye
kanye.name = "Yeezus"
# sets name to "Yeezus"
kanye.save
# saves the model to the database
```

> **Note:** Should be noted that when we set the attribute using a setter, it doesn't automatically save to the database, it's not until we call `.save` on the object that it saves to the database

---

To get all of the artists we use `.all`:

```ruby
Artist.all
```

---

We can also find an artist by its ID using `.find`:

```ruby
Artist.find(1)
```

<!-- SQL for find  -->
<details>
<summary>This would roughly translate to `SQL` code</summary>
<code>
SELECT * FROM artists WHERE id = 1 LIMIT 1;
</code>
</details>

<br>

---
Additionally we can also find an artist by an attribute using `.find_by`:

```ruby
Artist.find_by(name: "Elvis Presley")
```

> Note that find_by only returns the first object that meets the requirements of the arguments

---

If you want all artists that meet a certain query then we use `.where`:

```ruby
Artist.where(nationality: "American")
```

---

We can also update attributes and save at the same time using the `.update` method:

```ruby
elvis = Artist.find_by(name: "Elvis Presley")
elvis.update(nationality: "Canadian")
```

> If we want to update the attribute and not save we would have to use something from this [post](http://www.davidverhasselt.com/set-attributes-in-activerecord/)

---

Finally we can also just destroy/delete rows in our database with the `.destroy` method:

```ruby
kanye = Artist.find_by(name: "Yeezus")
kanye.destroy
# goodbye kanye you're gone forever
```

<span class="non-slide"></span><span class="non-slide"></span>
<span class="non-slide"></span><span class="non-slide"></span>
<span class="non-slide"></span><span class="non-slide"></span>

> This is exciting stuff by the way, imagine, while we do these things, that our artists model is instead a post on Facebook, or a comment on Facebook. So the next time you comment on someone's Facebook page you have an idea now of whats happening on the database layer. Maybe not the whole picture, but you have an idea. We're going to build on that idea in the coming week and half, and thats really exciting.

### You Do: Methods - Tunr (15 min / 1:30)

We will use `byebug` in your ruby app to test out ActiveRecord class and instance methods.

> Don't forget to require 'byebug' when you want to use `byebug` in your program.

[Part 1.1 - Use Your Artist Model](https://github.com/wdi-sg/activerecord-intro-exercise#part-11---use-your-artist-model)

### Resources
- [Active Record Basics](http://guides.rubyonrails.org/active_record_basics.html)
- [Active Record Query Interface](http://guides.rubyonrails.org/active_record_querying.html)
- [Tunr - Solution Code (zip)](https://github.com/ga-wdi-exercises/tunr-active-record/archive/v1.2.zip)

### Appendix

---

#### Instance vs Class Methods
| method              | instance | class    |
|---------------------|:--------:|:--------:|
| .new                |          |     x    |
| .save               |     x    |          |
| .create             |          |     x    |
| attribute accessors |     x    |          |
| .all                |          |     x    |
| .find               |          |     x    |
| .find_by            |          |     x    |
| .where              |          |     x    |
| .update             |     x    |          |
| .destroy            |     x    |          |
| .destroy_all        |          |     x    |

---

#### Conventions in AR
|description      | Rule    |
|-----------------|---------|
|table names      |snake case and plural|
|model file names |snake case and singular|
|model definition |Camel case and singular|
|argument for has_many| snake case and plural|
|argument for belongs_to| snake case and singular|
|more to follow....|||
