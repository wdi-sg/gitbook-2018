# CRUD in Express

CRUD is an acronym that stands for Create, Read, Update, Destroy. These are the basic operations that you can perform on data. Most sites you interact with on the internet are CRUD sites. Almost everything you do on the web is a CRUD action: Creating a user (create), Listing comments (read) to editing your profile (update), to deleting a video you uploaded on youtube (destroy).

[Formal definition on wiki](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete)

#### RESTful Routing

On the web the best practice for CRUD actions uses something called RESTful routing. The basic idea is your route (URL) should relate to the type of item you are interacting with, as well as the action you'll be performing to change/view the state of the item(s).

So if you were CRUDing Animals the route would be `/animals` and the following routes would form a full set of CRUD RESTful routes

| VERB | URL | Action (CRUD) | Description |
|------|-----|---------------|-------------|
| GET | /animals | Index (Read) | lists all animals |
| GET | /animals/1 | Show (Read) | list information about a specific animal (id = 1) |
| POST | /animals | Create | creates an animal with the POST payload data |
| PUT | /animals/1 | Update | updates the data for a specific animal (id = 1) |
| DELETE | /animals/1 | Delete (Destroy) | deletes the animal with the specified id (1) |

[Read more on wiki](http://en.wikipedia.org/wiki/Representational_state_transfer)

## CRUD in action

For now we're just going to focus on POST and GET (Create and Read)

**Backend data store**

For this example, we'll be using a JSON object as our data store. In the root of the project, create a JSON file with the following contents:

```json
[
  {
    "name":"Neko",
    "type":"cat"
  },
  {
    "name":"Fido",
    "type":"dog"
  },
  {
    "name":"Mufasa",
    "type":"lion"
  },
  {
    "name":"Tony",
    "type":"tiger"
  },
  {
    "name":"Kuma",
    "type":"bear"
  }
]
```

### Index / Read (GET)

Index is a route (URL) that lists all items of a specific type. It is a GET request to (in this example) `/animals`.

**Index route -- in index.js**

```js
// Include fs (short for filesystem) at the top. No need to install via npm
const fs = require('fs')

// Express index route for animals (lists all animals)
app.get('/animals', function(req, res) {
  let animals = fs.readFileSync('./data.json')
  animals = JSON.parse(animals)
  res.send('animals/index', {myAnimals: animals})
})
```

In the above example we load the `animals/index.ejs` view and pass it `animals` as `myAnimals`. This means in the index.ejs file we can access myAnimals directly.

**Index view -- in /views/animals/index.handlebars**

```handlebars
<ul>
  {{#each myAnimals}}
  <li>{{this.name}} is a {{this.type}}</li>
  {{/each}}
</ul>
```

Here we are looping through the `myAnimals` array passed from the route and
displaying a list of all of the items.

**output would be something like this:**

* Neko is a cat
* Fido is a dog
* Mufasa is a lion
* Tony is a tiger
* Kuma is a bear

### Show / Read (GET)

Show is a route that displays a single item of a specific type. It is GET
request to (in this example) `/animals/1`

**Show route -- in index.js**

```js
//express index route for animals (lists all animals)
app.get('/animals/:idx', function(req, res) {
  // get animals
  let animals = fs.readFileSync('./data.json');
  animals = JSON.parse(animals);

  //get array index from url parameter
  let animalIndex = parseInt(req.params.idx)

  //render page with data of the specified animal
  res.render('animals/show', {myAnimal: animals[animalIndex]})
});
```

In the above example we load the `animals/show.handlebars` view and pass it a specific
item from the `animals` array as `myAnimal`. We use the :idx url parameter to
specify which animal to display. Again, this means in the show.ejs file we can
access myAnimal directly.


```handlebars
{{ myAnimal.name }} is a {{ myAnimal.type }}.
```

Display the animal based on the url. With the example `/animals/1` we would be
displaying the 2nd item in the array (index starting at 0) and get output like
this...

Fido is a dog.

### Create (POST)

To create an item (animal in this example) we need to make a POST to the url `/animals` with the data about that animal. We do this using a form with the method attribute set to `post` and the action set to the create url.

**frontend - form html**
```html
<form method="POST" action="/animals">
  <label for="animalType">Type</label>
  <input type="text" name="type">

  <label for="animalName">Name</label>
  <input id="animalName" type="text" name="name">

  <input id="animalType" type="submit">
</form>
```

When the above form is submitted it will make a `POST` to the url `/animals` with the data contained in the form fields.

To receive this data we need to create a `POST` route in express and use the `body-parser` npm module to receive that data. But first, let's set up the `body-parser` module!


### BodyParser

Parsing parameters from a form needs an external module called `body-parser`.

```bash
yarn add body-parser
```

**index.js**
```js
const express = require('express')
const bodyParser = require('body-parser')
const app = express()

// tell your app to use the module
app.use(bodyParser.json())
app.use(bodyParser.urlencoded({
  extended: true
}))
```

Note that we set an attribute `extended` to `true` when telling our app to use the body parser. This attribute determines which library is used to parse data. Discussion on extended [here](http://stackoverflow.com/questions/29175465/body-parser-extended-option-qs-vs-querystring).

Now, if we try to add this backend route, calling `req.body` should contain the form input.

**backend - express route**
```js
app.post('/animals', function(req, res) {
  //debug code (output request body)
  console.log(req.body);
  res.send(req.body);
});
```

Form data is passed as *payload* of the request. Every field that has a name will
be included in that payload and it is sent as form encoded text. When
`body-parser` is installed it automatically **parses** the form body into a
javascript object that we can use and it stores it in `req.body` so we can use it. All of this is done as middleware, which we just configured.

In the above example we could access the animal type form field by using
`req.body.type` and the name field by using `req.body.name`. This correlates
directly to the names given to the form fields in the **form html** above.

Generally, the code in the **express route** would contain code that would
**CREATE** an item in a database and redirect the user to a route with a confirmation
message of some sort or just back to the index route. For this example we're
going to use the JSON file created above to store our data. This will involve three steps:

* Reading the JSON file
* Pushing the new animal to the object
* Writing the new JSON file

```js
app.post('/animals', function(req, res) {
  // read animals file
  let animals = fs.readFileSync('./data.json');
  animals = JSON.parse(animals);

  // add item to animals array
  animals.push(req.body);

  // save animals to the data.json file
  fs.writeFileSync('./data.json', JSON.stringify(animals));

  //redirect to the GET /animals route (index)
  res.redirect('/animals');
});
```

#### Additional: Show / Read (GET) with a Form

There may be instances where you need to GET animals, but you don't want them all. A good example is filtering animals with a specific name via a search bar.

In these cases, you don't want to use a POST action, because POST is reserved for creating new resources. Instead, we can create another form with a GET method, and read the data via a **querystring**.

**Animals index page**

```html
<form method="GET" action="/animals">
  <label for="nameFilter">Filter by Name</label>
  <input id="nameFilter" type="text" name="nameFilter">
  <input type="submit">
</form>
```

Note that this form will submit to the same page. We'll need to see if there's a querystring, then filter the animals if one is present.

```js
app.get('/animals', function(req, res){
  let animals = fs.readFileSync('./data.json');
  animals = JSON.parse(animals);

  let nameFilter = req.query.nameFilter;

  if (nameFilter) {
    animals = animals.filter(function(animal) {
      return animal.name.toLowerCase() === nameFilter.toLowerCase();
    });
  }

  res.render('animals/index', {myAnimals: animals});
});
```

### Forms

So far we've learnt to send actions using HTTP Methods, PUT for an update and DELETE for a destroy action. There is a problem however. Browsers currently only support GET and POST, so whenever we send a form it will actually be sent using POST. To overcome this we need to use the [Method Override](https://www.npmjs.com/package/method-override) Package. This will allow us to hide some information in the form that when received and processed in Express will change the method to the correct one. Here's an example

```js
var express = require('express')
var methodOverride = require('method-override')
var app = express()

// override with POST having ?_method=DELETE
app.use(methodOverride('_method'))
```
Example call with query override using HTML <form>:
```html
<form method="POST" action="/resource?_method=DELETE">
  <button type="submit">Delete resource</button>
</form>
```


## Controllers

We have been placing all routes into `index.js` when creating a Node/Express app, but this can get cumbersome when dealing with many routes. The solution is to separate routes into separate files and attach the routes to the Express router.

**index.js**

```js
const animalsCtrl = require("./controllers/animals")
app.use("/animals", animalsCtrl);
```

**controllers/animals.js**

```js
const express = require("express");
const router = express.Router();

router.get("/", function(req, res) {

});

module.exports = router;
```

Note that the routes should be defined *relative* to the definition in `app.use`. For example, the route defined above in `controllers/animals.js` will be `http://localhost:3000/animals`.
