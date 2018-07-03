## Passport Auth Setup

Passport is what's refered to as **express middleware**- a library that changes the way request and response are handled, and show up in your controllers.

When we are done setting it up we will have some useful things in request and response to use when we are dealing with authentication.

### Setup:
We will be changing about 7 files in our app in order to enable passport.
See the full set of changes here: [https://github.com/wdi-sg/express-reference/compare/sql-models...passport?expand=1](https://github.com/wdi-sg/express-reference/compare/sql-models...passport?expand=1)

#### yarn add

```
npm install add passport
npm install add passport-local
npm install add cookie-session
```

### User Signup

#### index.js

[https://github.com/wdi-sg/express-reference/blob/16522da0372f5125fe8508346792d4c217d620a6/index.js](https://github.com/wdi-sg/express-reference/blob/16522da0372f5125fe8508346792d4c217d620a6/index.js)

We need to require and configure all the new libraries we'll be using.

```js
const cookieSession = require('cookie-session');
const passport = require('passport');
const expressSession = require('express-session');
const pgSessionStore = require('connect-pg-simple')(expressSession);
```

After we initialize express we need this configuration:

```js
app.use(cookieParser('MySecret'));
app.use(cookieSession({
  secret:'MySecret'
}));

app.use(passport.initialize());
app.use(passport.session());
```

Change routes to pass in a reference to passport

```
require('./routes')(app, db, passport);
```

#### models/user.js

[https://github.com/wdi-sg/express-reference/blob/16522da0372f5125fe8508346792d4c217d620a6/models/user.js](https://github.com/wdi-sg/express-reference/blob/16522da0372f5125fe8508346792d4c217d620a6/models/user.js)

Make sure you have a method in your model that will get the user by name.

```js
const getByName = (name, callback) => {
  // set up query
  const queryString = 'SELECT * from users WHERE name=$1';
  const values = [name];

  // execute query
  dbPoolInstance.query(queryString, values, (error, queryResult) => {
    // invoke callback function with results after query has executed
    callback(error, queryResult);
  });
};
```

Remember to add it to your exported object.

```js
return {
  getByName,
  create,
  get,
  login
}
```

Change the `create` method to return the full user (not just the id)

```js
const queryString = 'INSERT INTO users (name, email, password) VALUES ($1, $2, $3) RETURNING *';
```

#### routes.js

[https://github.com/wdi-sg/express-reference/blob/16522da0372f5125fe8508346792d4c217d620a6/routes.js](https://github.com/wdi-sg/express-reference/blob/16522da0372f5125fe8508346792d4c217d620a6/routes.js)

Now let's setup the passport route for signup / user creation

Passport doesn't support this out of the box, so we need to name a new strategy for sign up.
We are going to need this name again in the controller.

```js
let namedStrategy = 'local-signup';
```

Configure the behavior of the signup when it's done processing (success and faliure)

```js
signupAuthConfig = {
  successRedirect : '/',
  failureRedirect : '/users/new'
};
```

Create passport's route callback by calling `passport.authenticate` and set it to the route

```js
let signupCallback = passport.authenticate(namedStrategy, signupAuthConfig)

app.post('/users', signupCallback);
```

#### controller/user.js

[https://github.com/wdi-sg/express-reference/blob/16522da0372f5125fe8508346792d4c217d620a6/controllers/user.js](https://github.com/wdi-sg/express-reference/blob/16522da0372f5125fe8508346792d4c217d620a6/controllers/user.js)

Let's setup the controller now that we have set the routes file.

Set the behavior of what happens when passport is done processing the incoming request
This replaces what we think of as the route callback.
All of the important signup params are explicitly passed in - we specify that in the object below.
```
const signupVerifyCallback = (request, name, password, done) => {

  db.user.create(request.body, (error, queryResult) => {

    let user = queryResult.rows[0];

    console.log("user:", user);

    // return the user and passport will set everything in the session
    return done(null, user);

  });
};
```

When we are done with this callback, the passed in user will be put into the session.
> session is a fancy cookie. We encrypt it with a secret and store it in a cookie.
> the more secure version is to store the session on the server. See below on how to store the entire session in the database

We need to tell passport how to store things.



[This line in the `index.js` file](https://github.com/wdi-sg/express-reference/blob/16522da0372f5125fe8508346792d4c217d620a6/controllers/user.js#L54) tells passport to use session to store the user so we can access it later.


This line in the `index.js` config(aobve) tells express to use the session library and sets the secret.

```js
app.use(cookieParser('MySecret'));
app.use(cookieSession({
  secret:'MySecret'
}));
```

Serialize will happen every time a user logs in or registers.
We explicitly set each key of the object so that we don't accidentally include the user's hashed password from the DB. That would be insecure.

```js
passport.serializeUser((user, done) => {

  let serializedUser = {
    id: user.id,
    name: user.name,
    email: user.email
  };

  done(null, serializedUser);
});
```

Deserialize will happen every time we get a request. The user will be deserialized into the request object.

```js
passport.deserializeUser((user, done) => {
  done(null, user);
});
```

Set some configs for the fields to be passed in, as well as session settings.

```
const signupLocalStrategyConfig = {

    // by default, local strategy uses username and password, we could override with email
    // these are reffering to keys in the request params
    usernameField : 'name',

    passwordField : 'password',

    session: true, //let's use session to store the user

    passReqToCallback : true
};
```

Create the strategy and tell express / passport to use it.

```
const signupLocalStrategy = new LocalStrategy(signupLocalStrategyConfig, signupVerifyCallback);
```

Give the same name for the signup strategy we defined in `routes.js`

```js
let namedStrategy = 'local-signup';

passport.use(namedStrategy, signupLocalStrategy);
```

#### index.js / root route

This is not neccessarily in root route but is wherever your root route is being rendered from.

[https://github.com/wdi-sg/express-reference/blob/16522da0372f5125fe8508346792d4c217d620a6/index.js](https://github.com/wdi-sg/express-reference/blob/16522da0372f5125fe8508346792d4c217d620a6/index.js)

Let's test that we have done everything correctly.

In the root route let's make some changes so that we know we are logged in.

1. passport gives us a new property of request `user`- it's the object we serialized when we registered- console log it so that we can see the value when we request the root route

> notice that if you have registered that `request.user` is the user we defined in `serializeUser` if you are not yet registered then it will be `undefined`.

```js
console.log("request user", request.user );
```

We can add 2 new keys into the template:

  1. the boolean for if the current requester is logged in / authenticated
  1. the entire user in the request / session

```js
let context = {
  isAuthenticated: request.isAuthenticated(),
  user:request.user,
  pokemon: queryResult.rows
};

response.render('home', context);
```

Make some changes to the root route template to display some stuff conditionally.

[https://github.com/wdi-sg/express-reference/blob/16522da0372f5125fe8508346792d4c217d620a6/views/home.handlebars](https://github.com/wdi-sg/express-reference/blob/16522da0372f5125fe8508346792d4c217d620a6/views/home.handlebars)

```
<div>
{{#if isAuthenticated}}
  <form action="/users/logout" method="POST">
    <input type="submit" value="logout, {{user.name}}"/>
  </form>
{{else}}
  <a href="/users/login">login</a>
  <a href="/users/new">signup</a>
{{/if}}
</div>
```

### User Login

Now we need to be able to log the user out and log them back in.

#### controller/user.js - logout

[https://github.com/wdi-sg/express-reference/blob/16522da0372f5125fe8508346792d4c217d620a6/controllers/user.js#L164](https://github.com/wdi-sg/express-reference/blob/16522da0372f5125fe8508346792d4c217d620a6/controllers/user.js#L164)

Passport provides a logout method to response.
Put that in your logout route.

```
request.logout();
```

#### routes.js - login

Set passport behavior at the routes level for when the authorization request succeeds / fails.

```
const localStrategyAuthConfig = {
  successRedirect: '/',
  failureRedirect: '/users/login',
  failureFlash: false,
  successFlash: false
};
```

The local strategy is the default passport authorization strategy. We don't have to name it as carefully as the signup strategy.
Call `passport.authenticate` to get the passport function that handles the express callback.

```
let localStrategy = 'local';
let loginCallback = passport.authenticate(localStrategy, localStrategyAuthConfig);
app.post('/users/login', loginCallback);
```

#### controller/user.js - login

[https://github.com/wdi-sg/express-reference/blob/16522da0372f5125fe8508346792d4c217d620a6/controllers/user.js#L75](https://github.com/wdi-sg/express-reference/blob/16522da0372f5125fe8508346792d4c217d620a6/controllers/user.js#L75)

Set the behavior of passport after the user posts to the login route.
`loginVerifyCallback` is called by passport after it recieves the request.
When we are done call done and pass in the user we matched from the DB.

> That user will get serialized into the session.

The behavior we specified in `routes.js` for login will happen after done.
If the user successfully logged in and was verified the response header will have the session cookie.

```js
const loginVerifyCallback = (request, name, password, done) => {
  console.log( "about to make db login query", name, password );

  db.user.getByName(name, (error, queryResult) => {

    if (error) { return done(error); }

    if (queryResult.rows.length <= 0) {
      return done(null, false);
    }

    let user = queryResult.rows[0];

    console.log( "comparing", user, password );

    let bcCompare = bcrypt.compareSync(password, user.password);

    if (!bcCompare) {
      return done(null, false);
    }

    // return the user and passport will set everything in the session
    // this goes to serialize user
    return done(null, user);
  });
};
```

Set the parameters to the login callback

```
const localStrategyLoginConfig = {
  passReqToCallback: true,
  usernameField: 'name',
  passwordField: 'password'
};
```

Tell passport to use the callback.

```
const localStrategy = new LocalStrategy(localStrategyLoginConfig, loginVerifyCallback);
passport.use(localStrategy);
```

#### With postgres as the session store:

https://github.com/wdi-sg/express-reference/compare/sql-models...ac7eb3acc1eaa2e3fbb4de2e76accf43fc0cd9ab?expand=1
