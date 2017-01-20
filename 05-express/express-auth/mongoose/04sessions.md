# User Sessions

We'll need to...

1. Install `express-session` middleware
2. Setup the session module

In order to save whether the user is logged in or not, we'll need to create a persistent session for each user. Each session will associate data with a user, and each user will be mapped to their data via a value stored in a cookie. Instead of directly altering the session, we'll use a third-party module called Passport to do it for us.

## Install express-session

```
npm install --save express-session
```

## Setup the session module

**index.js**

```js
// at the very top, require express-session
var session = require('express-session');

/*
 * setup the session with the following:
 *
 * secret: A string used to "sign" the session ID cookie, which makes it unique
 * from application to application. We'll hide this in the environment
 *
 * resave: Save the session even if it wasn't modified. We'll set this to false
 *
 * saveUninitialized: If a session is new, but hasn't been changed, save it.
 * We'll set this to true.
 */
app.use(session({
  secret: process.env.SESSION_SECRET,
  resave: false,
  saveUninitialized: true
}));
```

### What's this environment thing?
Environment variables are values that exist in a computer's current environment. They can affect how a computer runs, or how certain commands are executed.

You create them direct from your terminal. Example:
```bash
TEST=testvalue
echo $TEST
```

But they'll disappear as soon as the Terminal Session Ends. You could save them in your .bash_profile or .zshrc but then you'd have clashes across different Apps. The best solution is **add them to a .env file** in the project directory root that we can load when we start our node app.

Remember to make sure that .env is included in your .gitignore file. We use Environment Variables for hiding secrets, so you don't want to accidentally share them on Github.

**.env**

```
SESSION_SECRET=happycoding
```

There are many ways we can get the .env file to load one of the easiest is to use a package that does it for us - [dotenv](https://www.npmjs.com/package/dotenv).

First we have to install it
```
npm install dotenv --save
```

We then require and config at the top of our app. Silent tells it not to give us an errors if the file cannot be found - useful for when we deploy our code to Heroku.
```js
require('dotenv').config({ silent: true })
```

## Session finished

To verify that this session works, we'll start setting up Passport and login functionality in the next section. There are no tests associated with the sessions right now.

Additionally, you'll need to run your app and tests using `foreman` in order to read the environment variable.
