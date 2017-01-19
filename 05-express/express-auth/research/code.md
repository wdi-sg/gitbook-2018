#Lets code!

Now we will be implementing the topics you just researched. Come up with a working example, and be ready to teach everyone.

##Bcrypt

Implement code to use bcrypt to encrypt a plain text string AND then use `.compare()` to check it.

This can be done in a **stand alone .js file**. Be aware that bcrypt takes time so it uses a callback.

You may want to checkout the `bcrypt` package.

##Database Validations (Mongoose or Sequelize)

Create a Model with LOTS of validations, to demonstrate the possibilities. Test that it works by sending a POST request in postman/curl and return the errors.

##Database Hooks (Mongoose or Sequelize)

Create a Model and try out the various Hooks to change stuff at the various stages of the Lifecycle e.g. before and after create, update, destroy.

##Sessions

Use sessions to implement a back button for a simple App. The link should be on every page and and should navigate back to the page the user was previously on.

You may want to checkout the `express-session` package.

##Middleware

Create a middleware for the Daily Planet that adds a function `.log` to your `req` object. It should be created using `app.use()`. In any route, you should be able to call `req.log()`.

`req.log` should take 1 parameter that is the message to log. It should output

* The current date/time
* The current route's url
* The message provided

**usage example**

```js
app.get('/articles/:id', function(req, res) {
  req.log('loading an article');
});

//output...
//4/8/2015, 10:18:32 AM   /articles/4   loading an article
```
