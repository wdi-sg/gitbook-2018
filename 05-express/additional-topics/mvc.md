# Model View Controller

## Model

### db.js
Configs for your db and the model files:

```js
const pg = require('pg');
const pokemon = require('./models/pokemon');

configs = {
  user: 'akira',
  host: '127.0.0.1',
  database: 'pokemons',
  port: 5432
};


const pool = new pg.Pool(configs);

pool.on('error', function (err) {
  console.log('idle client error', err.message, err.stack);
});

module.exports = {
  /*
   * ADD APP MODELS HERE
   */
  pokemon: pokemon(pool),

  // get a reference to end the connection pool at server end
  pool:pool
};
```

### models/pokemon.js

Export model functions as a module
```js
module.exports = (dbPoolInstance) => {

  // `dbPoolInstance` is accessible within this function scope
  return {
    create: (pokemon, callback) => {
      // set up query
      const queryString = `INSERT INTO pokemons (name, num, img, weight, height)
        VALUES ($1, $2, $3, $4, $5)`;
      const values = [
        pokemon.name,
        pokemon.num,
        pokemon.img,
        pokemon.weight,
        pokemon.height
      ];

      // execute query
      dbPoolInstance.query(queryString, values, (err, queryResult) => {
        // invoke callback function with results after query has executed
        callback(err, queryResult);
      });
    },

    get: (id, callback) => {
      const values = [id];

      dbPoolInstance.query('SELECT * from pokemons WHERE id=$1', values, (error, queryResult) => {
        callback(error, queryResult);
      });
    }
  };
};
```


### index.js

Require the db file
```js
const db = require('./db');
```

Pass all of that stuff into routes
```js
require('./routes')(app, db);
```

### routes.js

Pass the db from routes into the controllers so that we can use them

```js
module.exports = (app, db) => {

  const pokemons = require('./controllers/pokemon')(db);

  app.get('/pokemon/:id', pokemons.get);
  app.get('/pokemons/:id/edit', pokemons.updateForm);
  app.post('/pokemons/:id/edit', pokemons.update);
  app.get('/pokemons/new', pokemons.createForm);
  app.post('/pokemons', pokemons.create);
  app.get('/pokemons/:id', pokemons.get);
};
```

## Controller

From `index.js` the routes are required.

The routes require the controller file.

For each route, pass in the controller function as the callback.

When the controller is included in the routes, db is passed in at that time.

```js
module.exports = (db) => {

  /**
   * ===========================================
   * Controller logic
   * ===========================================
   */
  const get = (request, response) => {
      // use pokemon model method `get` to retrieve pokemon data
      db.pokemon.get(request.params.id, (error, queryResult) => {
        // queryResult contains pokemon data returned from the pokemon model
        if (error) {
          console.error('error getting pokemon:', error);
          response.sendStatus(500);
        } else {
          // render pokemon view in the pokemon folder
          response.render('pokemon/Pokemon', { pokemon: queryResult.rows[0] });
        }
      });
  };

  const updateForm = (request, response) => {
      // TODO: Add logic here
  };

  const update = (request, response) => {
      // TODO: Add logic here
  };

  const createForm = (request, response) => {
    response.render('pokemon/new');
  };

  const create = (request, response) => {
      // use pokemon model method `create` to create new pokemon entry in db
      db.pokemon.create(request.body, (error, queryResult) => {
        // queryResult of creation is not useful to us, so we ignore it
        // (console log it to see for yourself)
        // (you can choose to omit it completely from the function parameters)

        if (error) {
          console.error('error getting pokemon:', error);
          response.sendStatus(500);
        }

        if (queryResult.rowCount >= 1) {
          console.log('Pokemon created successfully');
        } else {
          console.log('Pokemon could not be created');
        }
        // redirect to home page after creation
        response.redirect('/');
      });
  };

   // Export controller functions as a module
  return {
    get,
    updateForm,
    update,
    createForm,
    create
  };

}
```


### Timeline

#### App Startup
For our express MVC setup this happen in this order.

1. execute `index.js`
1. `db.js` is included
  1. `db.js` sets up the db connection pool
  1. `db.js` includes the model files
  1. the model file include is a function- it gets executed so that the models themselves get an instance of the pool connection.
  1. the connection pool and the model instances get returned to index.js
1.  the routes file gets included in index.js
  1. the routes file is a function, it is called with the express app instance and the db object
  1. every controller function gets included at the top of the routes file.
    1. the complete controller is a function. it gets executed so that it has access to the db object.
  1. each matching string route is in this file. for every route it gets a controller function
  1. (that controller function has already been passed the db instance


#### On request
1. Express responds to the callback set to that route.
1. The controller function has an instance of db pool to make a query with
1. The callback of the db query is written in the controller.
1. The response get sent back


