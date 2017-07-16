# MongoDB with Mongoose

## Objectives

* Update & destroy a model
* Initialize & create a new instance of a model
* Perform basic find queries
* Reference other documents in an instance of a model
* Work with embedded and referenced documents with Mongoose

## MongoDB + Mongoose

MongoDB is a document database for storing data. We can access MongoDB through Node by using an ORM called Mongoose. It's similar to other ORMs, but involves a little more setup.

## Setting up Mongoose in your app

Create a new Node app and install the relevant npm packages:

```bash
mkdir family-tree
cd family-tree

# setup npm
yarn init

# create index file
touch index.js
```

To use Mongoose in your Node app:

```bash
yarn add mongoose
```

With the package installed, lets use it - open index.js and setup your app:

```js

// Mongoose setup
const mongoose = require('mongoose')
mongoose.connect('mongodb://localhost/family-tree', {
  useMongoClient: true
})
mongoose.Promise = global.Promise
```

You can now execute all the mongoDB commands over the database `family-tree`, which will be created if it doesn't exist.

## Working with Models

#### Defining a Model

Like the ORMs we've worked with previously, Mongoose allows us to define models, complete with attributes, validations, and middleware (known as hooks in Sequelize, or callbacks in ActiveRecord). Let's make a model.

From within our family-tree app:

```
mkdir models
touch models/user.js
```

Now let's add:

```
const mongoose = require('mongoose')
const Schema = mongoose.Schema

// create a schema
const userSchema = new Schema({
  name: String,
  email: { type: String, required: true, unique: true },
  meta: {
    age: Number,
    website: String
  }
})
```

MongoDB is schemaless, meaning all the documents in a collection can have different fields, but for the purpose of a web app, often containing validations, we can still use a schema to cast and validate each type. Also note that we can have nested structures in a Mongoose model.

At the moment we only have the schema, representing the structure of the data we want to use. To save some data, we will need to make this file a Mongoose model and export it:

```
//in users.js
const mongoose = require('mongoose')
const Schema = mongoose.Schema

const userSchema = new Schema({
  name: String,
  email: { type: String, required: true, unique: true },
  meta: {
    age: Number,
    website: String
  }
})

const User = mongoose.model('User', userSchema)

// make this available to our other files
module.exports = User
```

Notice that you can use objects and nested attributes inside an object.

Here's a look at the datatypes we can use in Mongoose documents:

- String
- Number
- Date
- Boolean
- Array
- Buffer (binary)
- Mixed (anything)
- ObjectId

Also, notice we create the Mongoose Model with `mongoose.model`. Remember, we can define custom methods here - this would be where we could write a method to encrypt a password.

#### Creating Custom Methods

When defining a schema, you can add custom methods and call these methods on the models. These are instance methods. Let's write a `sayHello` function under our schema:

```
const mongoose = require('mongoose')
const Schema = mongoose.Schema

const userSchema = new Schema({
  name: String,
  email: { type: String, required: true, unique: true },
  meta: {
    age: Number,
    website: String
  }
})

userSchema.methods.sayHello = function () {
  return "Hi " + this.name
}

const User = mongoose.model('User', userSchema)

module.exports = User
```

Now we can call it by requiring the User model in index.js:

```
const User = require('./models/user');

// create a new user called Chris
const chris = new User({
  name: 'Chris',
  email: 'chris@gmail.com',
  meta: {
    age: 27,
    website: 'http://chris.me'
  }
})

```

Now run the app with `nodemon index.js` to see the result! You can define class methods in a similar manner by attaching the method to `.statics` instead of `.methods`

## Interacting with MongoDB's CRUD

Let's hope into an interactive shell and test out CRUD functionality. To do this, from our app directory, we'll have to type in `node` and then require our Models manually.

#### Create

We can first create a User and save it to the database using the `.save` method in Mongoose.

```
const newUser = new User({
  name: 'bob',
  email: 'bob@gmail.com'
})

// save the user
newUser.save(function (err) {
  if (err) {
    console.log(err);
    return;
  }
  console.log('User created!');
})
```

You can also call `.create` to *combine* creating and saving the instance.

```js
// create and save a user
User.create({ name: 'Emily', email: 'em@i.ly' }, function (err, user) {
  if (err) {
    console.log(err)
    return
  }
  console.log(user)
})
```

There is no "find or create" in Mongoose.

#### What about Read?

We can find multiple model instances by using the `.find` function, which accepts an object of conditions. There's also `.findOne` and `.findById` available.

```js
// Find All
User.find({}, function (err, users) {
  if (err) {
    console.log(err)
    return;
  }
  console.log(users)
})

// Find only one user
User.findOne({}, function (err, users) {
  if (err) {
    console.log(err)
    return
  }
  console.log(users)
})

// Find by email
User.find({ email: req.params.email }, function (err, users) {
  if (err) {
    console.log(err)
    return
  }
  console.log(users)
})

// Find by id
User.findById(req.params.id, function (err, users) {
  if (err) {
    console.log(err)
    return;
  }
  console.log(users)
})
```

Note that in the `.find` function, you can also use MongoDB queries such as `$gt`, `$lt`, `$in`, and others. Alternatively, there's a new `.where` syntax that can be used as well. [Documentation on Model.where can be found here](http://mongoosejs.com/docs/api.html#model_Model.where)

#### Update

Models can be updated in a few different ways - using `.update()`, `.findByIdAndUpdate()`, or `.findOneAndUpdate()`:

```js
// updates all matching documents
User.update({ name: 'brian' }, { meta: { age: 26 } }, function (err, user) {
  if (err) {
    console.log(err)
    return
  }
  console.log(user)
})

// updates one match only
User.findOneAndUpdate({ name: 'brian' }, { meta: { age: 26 } }, function (err, user) {
  if (err) {
    console.log(err)
    return
  }
  console.log(user)
})
```

#### Destroy

Models can be removed in a few different ways - using `.remove()`, `findByIdAndRemove()`, and `.findOneAndRemove()`.

```
// find all users with the name Brian and remove them
User.remove({ name: 'brian' }, function (err) {
  if (err) {
    console.log(err)
    return
  }
  console.log('Users deleted!')
})

// find the user with id 4 and remove it
User.findOneAndRemove({ name: 'brian' }, function (err) {
  if (err) {
    console.log(err)
    return
  }
  console.log('User deleted!')
})
```
