# Testing Node with Mocha/Chai

## Objectives

* Describe the importance of testing your code programmatically
* Use describe and assertion functions to do basic testing

## IMPORTANT NOTE

These concepts can be applied to any Express app. We'll be using our Taco app in this example, but feel free to use another.

https://github.com/wdi-sg/mocha-api-starter

## Mocha, Chai And Javascript Testing

We've now created several Express applications.  All these apps cover a single topic, so most of the time, they are quite small.  But when you create a larger application, the codebase will become bigger and more complex every time you add some features.  At some point, adding code in file A will break features in file B, and to avoid these "side-effects", we need to test our app.

To do so in Node, we will use two libraries: one to run the tests and a second one to run the assertions.

Mocha will be our testing framework. From the mocha Website:

_"Mocha is a feature-rich JavaScript test framework running on Node.js and the browser, making asynchronous testing simple and fun. Mocha tests run serially, allowing for flexible and accurate reporting, while mapping uncaught exceptions to the correct test cases."_


For assertions, we will use Chai. From the Chai website:

_"Chai is a BDD / TDD assertion library for node and the browser that can be delightfully paired with any javascript testing framework."_


To be able to make HTTP requests inside tests, we will use a framework called `supertest`.

## Let's Test!

### Setting up the app

Open the app provided and take a few minutes to explore the code and the routes being defined.

To test this app, we need to install a couple of dependencies.

First, let's install Mocha. We will install this globally only once, for convenience. Mocha is a command-line tool that can be run anywhere.

```bash
npm install -g mocha
```

Then we will install Chai, Supertest, and Mocha again using --save-dev

```bash
npm install chai supertest mocha --save-dev
```

**NOTE:** Saving Mocha as a development dependency does two things. First, we'll only have these tools in development environments. Second, we won't be relying on a user installing Mocha using the `-g` flag, thus guaranteeing that Mocha is installed. This will be handy for creating our own test scripts. Speaking of which...

#### Creating a test script

In order to run our tests by simply typing `npm test`, let's add a test script to `package.json`:

**package.json**

```
"scripts": {
  "test": "NODE_ENV=test node_modules/mocha/bin/mocha"
},
```

The script above will set the Node environment to `test`, which can be useful for handling test databases.

#### Files and Folders

Now that we're configured, let's set up our file and folder structure. All the tests will be written inside a folder `test` at the root of the app:

```bash
mkdir test
```

Then we will write the tests inside a file called `index_tests.js`:

```bash
touch test/index_tests.js
```

#### Let's write some tests

Open the file `index_tests.js`. We now need to require some dependencies at the top of this file:

```js
var expect = require('chai').expect;
var request = require('supertest');
var app = require('../index');
```

The code above imports Chai's `expect` assertions, as well as Supertest and our application.

All the tests need to be inside a `describe` function. `describe` functions are used to group tests. We can nest as many as we want. In our case, we'll use one `describe` block for our root route.

```js
describe('GET /', function() {
  //tests will be written inside this function
});
```

First, we will write a test to make sure that a request to `GET /` returns a HTTP status 200:

```js
describe('GET /', function() {
  it('should return a 200 response', function(done) {
    request(app).get('/')
    .expect(200, done);
  });
});
```

Now go in the command line and type `npm test`. You should get a message saying that you have 1 test passing.

Congrats, your test is passing!

> Note: You may see an error saying "Address already in use. This means nodemon is running your server somewhere else when your
tests also try to start the server. Either kill your nodemon server, or add this code around `app.listen()` to prevent your server
from starting twice:

```js
// prevent the app from starting twice if tests are running.
if (!module.parent) {
  app.listen(3000);
}
```

> If you see an error saying "TypeError: app.address is not a function", the following line needs to be added at the end of your main javascript file:

```js
 module.exports = app;
```

Every block of code that starts with `it()` represents a test.

The callback represents a function that Mocha will pass to the code so that the next test will be executed only when the current is finished and the `done` function is called - this allows tests to be executed once at a time.

## Verifying Tacos

Next, let's test the `tacos.js` controller.

#### before(), after(), beforeEach(), afterEach()

Mocha allows us to add hooks that will run before or after our test suites or each individual test. We can use these to avoid code duplication and also to ensure our test environment is consistent every time, for example, by dropping the database once connected, as illustrated using Mongoose below.

Let's create these tests in a separate file called **tacos_tests.js**

```js
var expect = require('chai').expect;
var request = require('supertest');
var app = require('../index');

before(function(done) {
  mongoose.connection.on('connected', function () {
    mongoose.connection.db.dropDatabase(done)
  })
});

```

#### GET /tacos

Testing `GET /tacos` will be similar as before.

```js
describe('GET /tacos', function() {
  it('should return a 200 response', function(done) {
    request(app).get('/tacos')
    .set("Accept", "application/json")
    .expect(200, done);
  });
});

```

We can write a test that verifies the response is an array:

```javascript
  // inside describe('GET /tacos', function() { ...
  it("should return an array", function(done){
    request(app).get('/tacos')
      .set("Accept", "application/json")
      .end(function(error, response){
        expect(response.body).to.be.an('array');
        done()
      })
    })
```

We can write another test that verifies the presence of a field in the response:

```javascript
  // inside describe('GET /tacos', function() { ...
  it("should return an object that has a field called 'name' ", function(done){
    request(app).get('/tacos')
      .set("Accept", "application/json")
      .end(function(error, response){
        expect(response.body[0]).to.have.property('name');
        done()
    })
  })
```

#### POST /tacos

Testing `POST /tacos` will require sending data.

```js
describe('POST /tacos', function() {
  it('should create a taco and return it', function(done) {
    request(app).post('/tacos')
    .set("Accept", "application/json")
    .send({
      name: 'Cheesy Gordita Crunch',
      amount: 6000
    })
    .expect(302, done);
  });
});
```

#### DELETE /tacos/:id

Testing `DELETE /tacos/:id` will not only require checking the status code, but also checking if the response has a message property with a value of "success". This will involve direct access to the response, which can be accessed through `supertest` using the `.end()` function.

```js
describe('DELETE /tacos/:id', function() {
  it('should return a 200 response on deleting a valid taco', function(done) {
    request(app).delete('/tacos/1')
    .end(function(err, response) {
      expect(response.statusCode).to.equal(200);
      expect(response.body).to.have.property('msg');
      expect(response.body.msg).to.equal('success');
      done();
    });
  });
});
```

Note how the `expect` assertions have additional functions that can be called, which make the lines read as if written in plain English. These types of tests fall under **Behavior-Driven Development**. BDD is an extension of TDD, and it's a testing process that revolves around testing and debugging specific behaviors. Many frameworks can be used to implement BDD. [Here's a great post with more information about TDD vs. BDD](https://www.toptal.com/freelance/your-boss-won-t-appreciate-tdd-try-bdd)

## Conclusion

We've covered the principles of testing in JavaScript, but Chai offers a lot of different expectations syntaxes. Check the [Chai Documentation](http://chaijs.com/api/)

- How does Mocha work with Chai to write tests in your JavaScript application?
- Describe how to configure your app to use Mocha and Chai.
