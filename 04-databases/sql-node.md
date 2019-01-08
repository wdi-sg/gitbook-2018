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

### Pairing Exercise:
Create a command line app that runs the movies `testdb` from the previous exercise, but inside of node.js. After each sql statement, output the result in a console.log, with a string that identifies that output.

```
mkdir nodepost
cd nodepost
npm init
npm install pg
touch index.js
```
