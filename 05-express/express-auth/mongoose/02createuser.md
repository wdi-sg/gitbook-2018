# Creating the User

We'll need to...

1. Install an additional dependency: `bcrypt`
2. Create the user model
3. Validate the user's name, email, and password
4. Hash the user's password before saving it to the database
5. Create methods to validate passwords and protect the password data

## Installing bcrypt

In order to hash passwords, we'll need to install `bcrypt`.

```
npm install --save bcrypt
```

## Creating the user model

Let's create a user with a name, email, and password. You can add more attributes later if you'd like.

**models/user.js**

```js
var mongoose = require('mongoose');
var bcrypt   = require('bcrypt');

var UserSchema = new mongoose.Schema({
  name:  { type: String },
  email: { type: String },
  password: { type: String }
});

module.exports = mongoose.model('User', UserSchema);

```

> This should pass the following test
>
> **Creating a User - should create successfully**

## Validate the user's name, email, and password

Now that we have a user, we want to limit the values we can assign to a user's name, email, and password. Here are some examples.

* User's email should be required, unique and a valid address
* User's name should be between 3-99 characters
* User's password should be required and between 8-99 characters

In order to do this, we can use Validations. __Note that by adding a `msg` within each validation, we'll be able to give a user-friendly message if a validation fails.__ This will be handled in our routes later.

[Mongoose Validation Documentation](http://mongoosejs.com/docs/validation.html)

**models/user.js**

```js

var emailRegex = /^([\w-\.]+@([\w-]+\.)+[\w-]{2,4})?$/;

var UserSchema = new mongoose.Schema({
  name:  {
    type: String,
    minlength: [3, 'Name must be between 3 and 99 characters'],
    maxlength: [99, 'Name must be between 3 and 99 characters'],
  },
  email: {
    type: String,
    required: true,
    unique: true,
    lowercase: true,
    match: emailRegex
  },
  password: {
    type: String,
    required: true ,
    minlength: [8, 'Password must be between 8 and 99 characters'],
    maxlength: [99, 'Password must be between 8 and 99 characters'],
  }
});

```

> This should pass the following tests
>
> **Creating a User - should throw an error on invalid email addresses**
>
> **Creating a User - should throw an error on invalid name**
>
> **Creating a User - should throw an error on invalid password**

## Hash the user's password before saving

Currently, we're saving user passwords as plain text. This is bad! Very bad!

* If someone gained access to our database, they would have a collection of emails and passwords. Since most people use the same password across different accounts, this can have drastic identity and legal consequences.
* *We the developers* shouldn't be able to see our users' passwords, for the same reasons above.

Therefore, we need to hash the password before it ever reaches the database. We can use a `beforeCreate` hook to do this automatically on every model's creation.

**models/user.js**

```js
// at the very top, require bcrypt
var bcrypt = require('bcrypt');

// ...
UserSchema.pre('save', function(next) {
   var user = this;

   // Only hash the password if it has been modified (or is new)
   if (!user.isModified('password')) return next();

   //hash the password
   var hash = bcrypt.hashSync(user.password, 10);

   // Override the cleartext password with the hashed one
   user.password = hash;
   next();
});
```

> This should pass the following test
>
> **Creating a User - should hash the password before save**

## Validating and Protecting the Password

Now that user passwords are hashed, we need to solve the last two problems with the user model.

* Comparing a password a user inputs to the user's hash in the database.
* Keeping the hash hidden

In order to perform these actions, we'll create two methods that can be called on user objects.

* To validate the password, we'll create an instance method called `validPassword` to accept a password as a parameter, then compare the password to the hash.
  * **Example**
  ```js
  user.validPassword('password'); // return true or false
  ```
* To hide the hash from the user object, we'll *override* the option of an instance method called `toJSON`, which will leave the hash out of the user's JSON object.
  * **Example**
  ```js
  user.toJSON(); // returns { name: 'Brian', email: 'bh@ga.co' }
  ```

**models/user.js**

```js
//...
UserSchema.methods.validPassword = function(password) {
  // Compare is a bcrypt method that will return a boolean,
  return bcrypt.compareSync(password, this.password);
};

UserSchema.options.toJSON = {
    transform: function(doc, ret, options) {
        // delete the password from the JSON data, and return
        delete ret.password;
        return ret;
    }
}
//...
```

> This should pass the following tests
>
> **User instance methods - validPassword - should validate a correct password**
>
> **User instance methods - validPassword - should invalidate an invalid password**
>
> **User instance methods - toJSON - should return a user without a password field**

## User Finished

Congrats, your user should be finished! Verify by running the user tests only. All tests should pass.

```
NODE_ENV=test node_modules/mocha/bin/mocha test/user.test.js
```
