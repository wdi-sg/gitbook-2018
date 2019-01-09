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

### Tips for working with your DB in node

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

### Pairing Exercise:
Recreate the same app as before, but using the DB setup techniques we just discussed. Copy the code you wrote from the other exercise.

```
mkdir nodepg
cd nodepg
npm init
npm install pg
touch index.js
touch tables.sql
touch seed.sql
createdb testdb -U USERNAME
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
