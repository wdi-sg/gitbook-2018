## Associations

Let's add a Song model to our code.

---

## Learning Objectives

- Utilize `has_many` and `belongs_to` to establish associations/relationships with Active Record
- Seed a database using Active Record


---

### Associations in Schema (5 / 1:35)

**NOTE:** In this section, we are reviewing our schema and how it reflects associations for our domain. We are NOT updating the schema file.

Looking back at our schema, in `schema.sql`:

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

Make note of the foreign key in `songs`

---

A good explanation for why we need to relate these things in ActiveRecord: 
http://guides.rubyonrails.org/association_basics.html

---

### Relationships in Active Record (5 min / 1:40)

Next we need to update the models to reflect the relationships in our application.

---

```ruby
# models/artist.rb
class Artist < ActiveRecord::Base
  has_many :songs
end
```

---

Now In our `song.rb` we have to reflect the association:

```ruby
class Song < ActiveRecord::Base
  belongs_to :artist
end
```
> note the plurality of `songs` and singularity of `artist`.  

---

We also need to include the `song.rb` file into our `app.rb` so in `app.rb` we need to add

```ruby
require_relative "song"
```

---
### You Do: Updating Class Definitions (5 min / 1:45)

[Part 1.2 - Create Your Song Model / Setup Associations](https://github.com/wdi-sg/activerecord-intro-exercise#part-12---create-your-song-model--setup-associations)

### Association Helper Methods (10 min / 1:55)

So we added some code, but we can't yet see the functionality it gives us.

Basically when we added those two lines of code `has_many :songs` `belongs_to :artist` we created some helper methods that allow us to query the database more effectively.

Lets create some objects in our `app.rb` so we can see what were talking about:
---

```ruby
Song.destroy_all
Artist.destroy_all

ratatat = Artist.create(name: "Ratatat", nationality: "American")
beatles = Artist.create(name: "The Beatles", nationality: "British")

# NOTE: you can pass an array to create for bulk creation
Song.create([
    {title: "Wildcat", album: "Classics", artist: ratatat },
    {title: "Loud Pipes", album: "Classics", artist: ratatat },
    {title: "Neckbrace", album: "LP4", artist: ratatat },
    {title: "Twist and Shout", album: "Please Please Me", artist: beatles},
    {title: "Hello, Goodbye", album: "Magical Mystery Tour", artist: beatles},
    {title: "Revolution", album: "The Beatles", artist: beatles}
  ])
```

---

> **Question:** Why do we need to use `.destroy_all`?

---
Now that we have this association, we can now easily query the database for the relevant records.

If we want to get all of The Beatles' songs or set The Beatles' songs, we can now write this code:

```ruby
beatles = Artist.find_by(name: "The Beatles")
beatles.songs
# will return all songs that belong to The Beatles, this is .songs being used as a getter method

beatles.songs = [Song.first, Song.last]
# this is .songs being used as a setter method
```
> note that when songs is being used as a setter method above, it actually changes the artist_id column for those songs to match The Beatles' primary ID. Any song that previously was assigned to The Beatles and not reassigned in the setter will now have an artist_id of nil

---

Alternatively if we wanted to get a song's artist we could write this code:

```ruby
loud_pipes = Song.find_by(title: "Loud Pipes")
loud_pipes.artist
# will return loud_pipes' artist, this is .artist being used as a getter method

beatles = Artist.last
loud_pipes.artist = beatles
loud_pipes.save
# this .artist being used as a setter method, and now loud_pipes's artist is the beatles.
```

---

We can also create new songs under a certain artist by doing the following:

```ruby
beatles.songs.create(title: "Hey Jude", album: "Beatles Chillout (Vol. 1)")
# this will create a new song that belongs to the Artist object named beatles.
```
> **Note** that we did not pass in an artist id above. Active Record is smart and does that for us.

---

### You Do: Association Helper Methods (10 min / 2:05)

[Part 1.3 - Use Your Model Assocations](https://github.com/wdi-sg/activerecord-intro-exercise#part-13---use-your-model-associations)

---

## Seeding a Database with ActiveRecord (10 min / 2:15)
Seeding a database is not all that different from the things we've been doing today. 

> What's the purpose of seed data?

We want some sort of data in our database so that we can test our applications.

---
Let's create a seed file in the terminal: `$ touch seeds.rb`

In our `seeds.rb` file, let's put the following code:

```ruby
require "bundler/setup" # require all the gems we'll be using for this app from the Gemfile. Obviates the need for `bundle exec`

require "pg"
require "active_record"
require "byebug"

require_relative "./song"
require_relative "./artist"

Artist.destroy_all
Song.destroy_all
# destroys existing data in database

chili_peppers = Artist.create(name: "Red Hot Chili Peppers", nationality: "American")
led = Artist.create(name: "Led Zeppelin", nationality: "British")

chili_peppers.songs.create([
    {title: "Can't Stop", album: "By The Way" },
    {title: "Scar Tissue", album: "Californication" },
    {title: "Californication", album: "Californication" },
    {title: "Dani California", album: "Stadium Arcadium" },
    {title: "Dark Necessities", album: "The Getaway"}
  ])

led.songs.create([
    {title: "Whola Lotta Love", album: "Led Zeppelin II" },
    {title: "Stairway to Heaven", album: "Led Zeppelin IV" },
    {title: "Kashmir", album: "Physical Graffiti" },
    {title: "Black Dog", album: "Led Zeppelin IV" },
    {title: "All My Love", album: "In Through the Out Door"}
    ])
```

Once we get rid of this duplicate CRUD code in `app.rb` we can just run this seed file once and know our data is good.

Now when we run our application with `ruby app.rb`, we enter into byebug with all our data loaded.

## Closing Review (15 min / 2:30)

Active Record is extremely powerful and helpful, and allows us to easily interface with the business models for our applications.

### Resources
- [Active Record Basics](http://guides.rubyonrails.org/active_record_basics.html)
- [Active Record Query Interface](http://guides.rubyonrails.org/active_record_querying.html)
- [Tunr - Solution Code (zip)](https://github.com/ga-wdi-exercises/tunr-active-record/archive/v1.2.zip)

### Appendix

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

#### Conventions in AR
|description      | Rule    |
|-----------------|---------|
|table names      |snake case and plural|
|model file names |snake case and singular|
|model definition |Camel case and singular|
|argument for has_many| snake case and plural|
|argument for belongs_to| snake case and singular|
|more to follow....|||
