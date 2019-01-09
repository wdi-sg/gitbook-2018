# CRUD Express SQL

Now we'll integrate our node.js code into an express app.

![3-Tier Express Application](images/3-tier-application.jpg)

#### RESTful Routing

We can also see that each HTTP action maps to a kind of SQL query.

| **URL**          | **HTTP Verb** |  **Action**| **SQL**     |
|------------------|---------------|------------|-------------|
| /photos/         | GET           | index      | SELECT      |
| /photos/new      | GET           | new        | N/A (SELECT)|
| /photos          | POST          | create     | INSERT      |
| /photos/:id      | GET           | show       | SELECT      |
| /photos/:id/edit | GET           | edit       | SELECT      |
| /photos/:id      | PATCH/PUT     | update     | UPDATE      |
| /photos/:id      | DELETE        | destroy    | DELETE      |

### pg pool
Pool is a functionality oif the `pg` library that manages connecting with the database when your app may try to connect multiple times.

We will be using the `pool` bolierplate to make SQL queries.

In your express config (at the top of the file)
```
const pg = require('pg');

// Initialise postgres client
const config = {
  user: 'akira',
  host: '127.0.0.1',
  database: 'pokemons',
  port: 5432,
});

const pool = new pg.Pool(config);

pool.on('error', function (err) {
  console.log('idle client error', err.message, err.stack);
});
```

Call `pool.query` as a replacement for `client.query`

```
const queryString = 'SELECT * from pokemon'

pool.query(queryString, (err, result) => {

  if (err) {
    console.error('query error:', err.stack);
    response.send( 'query error' );
  } else {
    console.log('query result:', result);

    // redirect to home page
    response.send( result.rows );
  }
});
```

### Pairing Exercise

create a new dir
```
mkdir poke-sql
```
cd into it
```
cd poke-sql
```
init npm
```
npm init
```
add express
```
npm install express
```
add pg
```
npm install pg
```

create your db
```
createdb pokemons -U akira
```

create a tables.sql
```
touch tables.sql
```

```
CREATE TABLE IF NOT EXISTS pokemons (
    id SERIAL PRIMARY KEY,
    name TEXT,
    phone VARCHAR(15),
    email TEXT
);
```

Use some seed data:
```
INSERT INTO pokemon
(name, img, weight, height)
VALUES
('Bulbasaur', 'http://www.serebii.net/pokemongo/pokemon/001.png', '6.9 kg', '0.71 m');

INSERT INTO pokemon
(name, img, weight, height)
VALUES
('Ivysaur', 'http://www.serebii.net/pokemongo/pokemon/002.png', '13.0 kg', '0.99 m');

INSERT INTO pokemon
(name, img, weight, height)
VALUES
('Venusaur', 'http://www.serebii.net/pokemongo/pokemon/003.png', '100.0 kg', '2.01 m');
```

run  the tables.sql and seed.sql
```
psql -d DATABASE_NAME -U USERNAME -f tables.sql
psql -d DATABASE_NAME -U USERNAME -f seed.sql
```

start your app
```
nodemon index.js
```

use this express starter code in index.js:

```
console.log("starting up!!");

const express = require('express');
const pg = require('pg');

// Initialise postgres client
const configs = {
  user: 'YOURUSERNAME',
  host: '127.0.0.1',
  database: 'pokemons',
  port: 5432,
};

const pool = new pg.Pool(configs);

pool.on('error', function (err) {
  console.log('idle client error', err.message, err.stack);
});

// Init express app
const app = express();

app.use(express.json());
app.use(express.urlencoded({
  extended: true
}));

// Set react-views to be the default view engine
const reactEngine = require('express-react-views').createEngine();
app.set('views', __dirname + '/views');
app.set('view engine', 'jsx');
app.engine('jsx', reactEngine);

app.get('/', (req, res) => {
  // query database for all pokemon

  // respond with text that lists the names of all the pokemons
  res.send('hello');
});

// boilerplate for listening and ending the program

const server = app.listen(3000, () => console.log('~~~ Tuning in to the waves of port 3000 ~~~'));

let onClose = function(){

  console.log("closing");

  server.close(() => {

    console.log('Process terminated');

    pool.end( () => console.log('Shut down db connection pool'));
  })
};

process.on('SIGTERM', onClose);
process.on('SIGINT', onClose);
```

#### Further
Write the code necessary to do an INSERT.

- write a get route that renders a form
- write a post route that does the insert statement

#### Further
Create the view files for the `/` root route.

#### Further
Create the `/pokemon/:id` route.
