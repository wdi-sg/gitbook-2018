# Login User with Passport

We'll need to...

1. Install dependencies, `passport` and `passport-local`
2. Configure Passport to use our user model
3. Initialize Passport to use our session module
4. Add login and logout functionality to the `auth` controller

## What is Passport?

From the [passport website](http://passportjs.org/docs):

"_Passport is authentication Middleware for Node. It is designed to serve a singular purpose: authenticate requests. When writing modules, encapsulation is a virtue, so Passport delegates all other functionality to the application. This separation of concerns keeps code clean and maintainable, and makes Passport extremely easy to integrate into an application._

_In modern web applications, authentication can take a variety of forms. Traditionally, users log in by providing a username and password._"

#### Strategies

The main concept when using passport is to register _Strategies_.  A strategy is a passport Middleware that will create some action in the background and execute a callback; the callback should be called with different arguments depending on whether the action that has been performed in the strategy was successful or not. Based on this and on some config params, passport will redirect the request to different paths.

Because strategies are packaged as individual modules, we can pick and choose what modules we need for our application. This logic allows the developer to keep the code simple - without unnecessary dependencies - in the controller and delegate the proper authentication job to some specific passport code. On a high-level, you can think of the passport module as authentication middleware the app uses and any passport strategy module (`passport-*`) as detailed authentication middleware that passport itself uses.


## Install passport and passport-local

We'll use Passport in order to provide login functionality, and `passport-local` in order to provide local user authentication.

```
npm install --save passport passport-local
```

## Configure Passport to use our user model

Create the Passport configuration inside of a folder called config.

**config/ppConfig.js**

```js
var passport = require('passport');
var LocalStrategy = require('passport-local').Strategy;
var User = require('../models/user');

/*
 * Passport "serializes" objects to make them easy to store, converting the
 * user to an identifier (id)
 */
passport.serializeUser(function(user, done) {
  done(null, user.id);
});

/*
 * Passport "deserializes" objects by taking the user's serialization (id)
 * and looking it up in the database
 */
passport.deserializeUser(function(id, done) {
  User.findById(id, function(err, user) {
    done(err, user);
  });
});

/*
 * This is Passport's strategy to provide local authentication. We provide the
 * following information to the LocalStrategy:
 *
 * Configuration: An object of data to identify our authentication fields, the
 * username and password
 *
 * Callback function: A function that's called to log the user in. We can pass
 * the email and password to a database query, and return the appropriate
 * information in the callback. Think of "cb" as a function that'll later look
 * like this:
 *
 * login(error, user) {
 *   // do stuff
 * }
 *
 * We need to provide the error as the first argument, and the user as the
 * second argument. We can provide "null" if there's no error, or "false" if
 * there's no user.
 */
passport.use(new LocalStrategy({
  usernameField: 'email',
  passwordField: 'password'
}, function(email, password, done) {
  User.findOne({ email: email }, function(err, user) {
    if (err) return done(err);

    // If no user is found
    if (!user) return done(null, false);

    // Check if the password is correct
    if (!user.validPassword(password)) return done(null, false);

    return done(null, user);
  });
}));

// export the Passport configuration from this module
module.exports = passport;
```

## Initialize Passport to use our session module

Now that we've created the configuration, we need to make our app *aware of its existence*. This can be done by requiring the configuration and including it as middleware.

**index.js**

```js
// require the configuration at the top of the file
var passport = require('./config/ppConfig');

// initialize the passport configuration and session as middleware
app.use(passport.initialize());
app.use(passport.session());
```

**IMPORTANT NOTE:** You must include the passport configuration **below your session configuration.** This ensures that Passport is aware that the session module exists.

## Add login and logout functionality

> Before continuing, verify that this test is passing
>
> **Auth Controller - GET /auth/login - should return a 200 response**

#### Login

Luckily, all of that configuration and middleware means a straightforward login route. Let's go ahead and add the POST route for login.

**controllers/auth.js**

```js
// require the passport configuration at the top of the file
var passport = require('../config/ppConfig');

router.post('/login', passport.authenticate('local', {
  successRedirect: '/',
  failureRedirect: '/auth/login'
}));
```

> This should pass the following tests
>
> **Auth Controller - POST /auth/login - should redirect to / on success**
>
> **Auth Controller - POST /auth/login - should redirect to /auth/login on failure**

#### Login after Signup

Ideally, we want to already be logged in after signup. We can modify the signup route to call the `passport.authenticate` function again.

**controllers/auth.js**

```js
router.post('/signup', function(req, res) {
  User.create({
    email: req.body.email,
    name: req.body.name,
    password: req.body.password
  }, function(err, createdUser) {
    if(err){
      // FLASH
      console.log('An error occurred: ' + err);
      res.redirect('/auth/signup');
    } else {
      // FLASH
      passport.authenticate('local', {
        successRedirect: '/'
      })(req, res);
    }
  });
});
```

#### Logout

Including the Passport configuration in our app means that logging out is really really easy. You can now call a function attached to `req` to log out. Let's implement the final route.

**controllers/auth.js**

```js
router.get('/logout', function(req, res) {
  req.logout();
  console.log('logged out');
  res.redirect('/');
});
```

> This should pass the following test
>
> **Auth Controller - GET /auth/logout - should redirect to /**

## Login/Logout Finished

Congrats, your login/logout functionality should be finished! Verify by running the auth tests only. All tests should pass.

```
NODE_ENV=test foreman run node_modules/mocha/bin/mocha test/auth.test.js
```

Now for one more section...
