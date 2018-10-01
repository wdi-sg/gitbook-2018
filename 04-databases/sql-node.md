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

---

### How to connect to the DB:
Configure the client to connect to the DB server

---

1. require the library
```
const pg = require('pg');
```

---

2. set all of the configuration in an object
```
const configs = {
  user: 'akira',
  host: '127.0.0.1',
  database: 'pokemons',
  port: 5432,
};
```

---

3. create a new instance of the client
```
const client = new pg.Client(configs);
```
(this is the client you will be using for this single web server instance)

---

### Using the client in your app
1. setup the query text and the values you want to work with
1. connect to the client, and after you connect, call the query method

```
client.connect((err) => {

  if( err ){
    console.log( "error", err.message );
  }

  client.query(text, values, (err, res) => {
    if (err) {
      console.log("query error", err.message);
    } else {
      console.log("result", res.rows[0]);
    }
  });

});
```

### Node.js SELECT
```
let queryText = 'SELECT * FROM students';

client.query(queryText, (err, res) => {
    if (err) {
      console.log("query error", err.message);
    } else {
      // iterate through all of your results:
      for( let i=0; i<res.rows.length; i++ ){
        console.log("result: ", res.rows[i]);
      }
    }
});
```
### Node.js INSERT

Most basic insert statement

```
let queryText = "INSERT INTO students (name, phone, email) VALUES ('scott', '(415) 111-6666', 'scott@email.com')";

client.query(queryText, (err, res) => {
    if (err) {
      console.log("query error", err.message);
    } else {
      console.log("done!");
    }
});
```

**insert with parameters** and **RETURNING**

Normally you will use the 2nd parameter of `client.query`- it allows you to pass in an array of values.

RETURNING allows you to get back the `id` of the thing you inserted.

```
let queryText = 'INSERT INTO students (name, phone, email) VALUES ($1, $2, $3) RETURNING id';

const values = ["chee kean", "63723625", "ck@ga.co"];

client.query(queryText, values, (err, res) => {
    if (err) {
      console.log("query error", err.message);
    } else {
      console.log("id of the thing you just created:", res.rows[0].id);
    }
});
```

---

## Development workflow with postgres: tables.sql, seed.sql & drop.sql

When working with your app it is helpful to be able to deal with all of your data easily.

We'll create a workflow to easily wipe away your development data and start fresh.

---

### Note: Create a database - easy way

Use postgres' createdb command- the `-U` flag is the username

```
createdb DATABASE_NAME -U USERNAME
```

Create a DB called pokemons
```
createdb pokemons -U akira
```

---

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

To run the file on the command line:
```
psql -d DATABASE_NAME -U USERNAME -f tables.sql
```

---

### Start the db with some dummy data - seed.sql
Write some lines of SQL that will populate the db with some small data.

This is normally things such as a first default user and other things you need to get up and running with your app.

It works the same way as `tables.sql` above, but it's only for data.

Inside of `seed.sql`:
```
INSERT INTO movies (title, description, rating) VALUES('Cars', 'a movie', 9);
```

That way we always have some dummy data to start out our app with.

Run the file:
```
psql -d DATABASE_NAME -U USERNAME -f tables.sql
```

---

### Clear away your changes:
In development it can be very useful to clear away the data you are writing into your db easily- just run this sql, then run your tables.sql above.

```
DROP DATABASE dbname;
```

---

#### Return the thing you created: `RETURNING`

```
client.query(insertQuery, (err, result) => {
  // inside of result is the newly inserted thing
```

Return the id of the result of the insert.
```
INSERT INTO movies (title, description, rating) VALUES('Cars', 'a movie', 9) RETURNING id;
```

Return the result of the insert.
```
INSERT INTO movies (title, description, rating) VALUES('Cars', 'a movie', 9) RETURNING *;
```

---

#### Date Data Type

When you set data type date in postgres, you get out a javascript Date object.

```
var d = new Date(); // this is a javascript date object.
```

Use `DEFAULT now()` to automatically have a `created_at` column in your database.

```
const createTableText = `
CREATE TEMP TABLE dates(
  date_col DATE DEFAULT now(),
  timestamp_col TIMESTAMP DEFAULT now(),
  timestamptz_col TIMESTAMPTZ DEFAULT now(),
  created_at TIMESTAMPTZ DEFAULT now()
);
`
// create our temp table
client.query(createTableText, (err, result) => {
      // read the row back out
    client.query('SELECT * FROM dates', (err, res) => {
      console.log(result.rows)
      // {
      // date_col: 2017-05-29T05:00:00.000Z,
      // timestamp_col: 2017-05-29T23:18:13.263Z,
      // timestamptz_col: 2017-05-29T23:18:13.263Z
      // }
    })
});
```

You can also use the plain js date data type to insert rows.
```
// insert the current time into it
const now = new Date()
const insertText = 'INSERT INTO dates(date_col, timestamp_col, timestamtz_col'
client.query(insertText, [now, now, now], (err, res) => {
  console.log( res.rows );
});
```
---

### Pairing Exercise:
Create a command line app that runs the previous exercise, but inside of node.js. After each sql statement, output the result in a console.log, with a string that identifies that output.

```
mkdir nodepg
cd nodepg
npm init
npm install pg
touch index.js
touch tables.sql
touch seed.sql
createdb your-test-db -U USERNAME
```
Use tables.sql to record the tables you need.

Example:
```
CREATE TABLE IF NOT EXISTS students (
    id SERIAL PRIMARY KEY,
    name TEXT,
    phone VARCHAR(15),
    email TEXT
);
```

Use seed.sql to put some starting dummy data in the DB.

Example:
```
INSERT INTO students (name, phone, email) VALUES ('hello', 'bye', 'monkey');
```

Using the syntax above, run your `index.js` script that runs all your sql commands.
```
node index.js
```

Use DROP to wipe it away and start again.
