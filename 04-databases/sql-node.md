## POSTGRES and Nodejs

We are now going to replace our JSON file with a SQL database.

Similarities:
- asynchronous
- still CRUD
- disk based storage

Differences:
- we can use `PRIMARY KEY`
- interact with server over the network
- better error handling / less possibility of error
  - what if someone moves or edits the file
  - what if two users both post the same thing
  - what if the name of the file changes

### How to connect to the DB:
Configure the client to connect to the DB server

1. require the library
```
const pg = require('pg');
```

2. set all of the configuration in an object
```
const configs = {
  user: 'akira',
  host: '127.0.0.1',
  database: 'pokemons',
  port: 5432,
};
```

3. create a new instance of the client
```
const client = new pg.Client(configs);
```
(this is the client you will be using for this single web server instance)

### Using the client in your app
1. setup the query text and the values you want to work with
1. connect to the client, and after you connect, call the query method

```
client.connect((err) => {

  if( err ){
    console.log( "error", err.message );
  }
  
  // the text variable holds the SQL query you want to execute
  let text = 'SELECT * from students';
  let values = [];

  client.query(text, values, (err, res) => {
    if (err) {
      console.log("query error", err.message);
    } else {
      console.log("result", res.rows[0]);
    }
    
    // if this is the last query, close the connection
    client.end();
  });

});
```

### string interpolation with values array
You can create a query with dynamic values in the string using the values array.

Each element in the array turns into a `$x` in the SQL query string.

1. Create a query that we will interpolate some values into:
```
let text = "INSERT INTO students (name, email, phone) VALUES( $1, $2, $3 )";
```

2. Create an array with the values we want to put into the string:
```
let values = ['akira', 'akira@foo.com', '(415)586-0370'];
```

3. Postgres will eventually (under the hood) create a string that looks like this:
```
INSERT INTO students (name, email, phone) VALUES( 'akira', 'akira@foo.com', '(415)586-0370')";
```

## Development workflow with postgres: tables.sql, seed.sql & drop.sql

### Note: Create a database - easy way

Use postgres' createdb command- the `-U` flag is the username

```
createdb DATABASE_NAME -U USERNAME
```

Create a DB called pokemons
```
createdb pokemons -U akira
```

### Save your DB tables:
When you want to add a table (including the first table in your DB we will be writing that sql in a file: **tables.sql**

We will be using one new piece of syntax to do that: `CREATE TABLE IF NOT EXISTS`
```
CREATE TABLE IF NOT EXISTS students (
    id SERIAL PRIMARY KEY,
    name TEXT,
    phone VARCHAR(15),
    email TEXT
);
```

So you can simply run the file each time you add a table, and only the uncreated tables will get created.

```
psql -d DATABASE_NAME -U USERNAME -f tables.sql
```

Example contents of a tables.sql file:
```
CREATE TABLE IF NOT EXISTS movies (
  id SERIAL PRIMARY KEY,
  title TEXT,
  description TEXT,
  rating INTEGER
);

CREATE TABLE IF NOT EXISTS actors (
  id SERIAL PRIMARY KEY,
  name TEXT,
  age INTEGER
);
```

### Start the db with some dummy data
Write some lines of SQL that will populate the db with some small data.

This is normally things such as a first default user and other things you need to get up and running with your app.

It works the same way as `tables.sql` above, but it's only for data.

Example contents of `seed.sql`:
```
INSERT INTO movies (title, description, rating) VALUES('Cars', 'a movie', 9);
INSERT INTO movies (title, description, rating) VALUES('Back to the Future', 'another movie', 9);
```

You would run it like:
```
psql -d DATABASE_NAME -U USERNAME -f seed.sql
```

### Clear away your changes:
In development it can be very useful to clear away the data you are writing into your db easily, without having to run the command in psql.

Save this snippet in a cleardb.sql file. **Warning: BE VERY CAREFUL ABOUT RUNNING IT**
The schemaname is usually the same name as your dbname.

#### drop.sql
From: [https://stackoverflow.com/a/36023359/271932](https://stackoverflow.com/a/36023359/271932)
```
DO $$ DECLARE
    r RECORD;
BEGIN
    FOR r IN (SELECT tablename FROM pg_tables WHERE schemaname = current_schema()) LOOP
        EXECUTE 'DROP TABLE IF EXISTS ' || quote_ident(r.tablename) || ' CASCADE';
    END LOOP;
END $$;
```

You would run it like:
```
psql -d DATABASE_NAME -U USERNAME -f drop.sql
```

### Pairing Exercise:
Create a command line app that runs the table manipulation SQL commands from the previous exercise, but inside of node.js. (That means we are still creating our databases and tables outside of node.js) After each sql satement, output the result in a console.log, with a string that identifies that line of output.

Use tables.sql to record the tables you need.

Use seed.sql to put some starting dummy data in the DB.

Run your `index.js` script that runs all your sql commands.

Use drop.sql to wipe it away and start again.
