# Views and Templates
---

### Views

We cannot keep using `response.send` to send a response. Ultimately, we'll want to send HTML files back to the client.

We want to have this page's HTML be different for each request. How do we do this??

---

### Templating with React

If we want to customize what's on the page? We're going to set up a template engine with **[React](http://reactjs.org)** and use that instead.

React is used on the front end of lots of sites, but it's __JSX__ component can also be used to simply create HTML.

We need to do a couple steps to get the template engine working.

---

First, install [`express-react-views`](https://github.com/reactjs/express-react-views)

````
npm install express-react-views react react-dom
```

Then, prepare this directory structure on your `node` project.

```
.
├── app.js
└── views
    ├── home.jsx

1 directories, 2 files
```

---

Once structure is setup, you can setup the `express` view engine to `jsx` in this manner.

```javascript
const express = require('express')
const app = express();


// this line below, sets a layout look to your express project
const reactEngine = require('express-react-views').createEngine();
app.engine('jsx', reactEngine);

// this tells express where to look for the view files
app.set('views', __dirname + '/views');

// this line sets react to be the default view engine
app.set('view engine', 'jsx');

app.get('/', (req, res) => {
  // running this will let express to run home.handlebars file in your views folder
  res.render('home')
})
```

---
### JSX

JSX is javascript and HTML. It looks like this:
```
var React = require('react');

class Home extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello</h1>
      </div>
    );
  }
}

module.exports = Home;
```

At first it just seems like a javascript syntax error, but what we are doing is running this file through a parser that will create HTML for us.

Everything between the `return` parentheses is going to be rendered into HTML.

### Templating

Templating with variables means we can pass in an object to the `.render` function and access those variables inside the `jsx` template. These variables are also called `context`

**index.js**

```js
const express = require('express')
const app = express();


// this line below, sets a layout look to your express project
const reactEngine = require('express-react-views').createEngine();
app.engine('jsx', reactEngine);

// this tells express where to look for the view files
app.set('views', __dirname + '/views');

// this line sets handlebars to be the default view engine
app.set('view engine', 'jsx');

app.get('/', (req, res) => {
  // giving home.jsx file an object/context with `name` as a property
  res.render('home', {name: "Sterling Archer"});
});

app.listen(3000);
```

---

then we need to update our `home.jsx` to use a templating variable.

**views/home.jsx**
```html
var React = require('react');

class Home extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, { this.props.name }!</h1>
      </div>
    );
  }
}

module.exports = Home;
```


The JavaScript being embedded is enclosed by the `{ }` tags.


---

#### Conditional Rendering
Sometimes we want to decide to render something based on a variable.

We can put this code directly above the return statement, or we could also write it in another function.
```
var React = require('react');

class Home extends React.Component {
  render() {

    let message = "welcome!";

    if( name.length > 5 ){
      messgae = "welcome! What a long name you have!";
    }

    return (
      <div>
        <h1>Hello, { this.props.name }!</h1>
        <h1>{ message }</h1>
      </div>
    );
  }
}

module.exports = Home;
```

#### Map
The ES6 array method `map` allows us to return an array.

We can use `map` to create HTML in a loop.

Say we have the following `context` given to our `jsx` template.
```js
var context = {
  people: [
    "Yehuda Katz",
    "Alan Johnson",
    "Charles Jolley"
  ]
}
```

```
var React = require('react');

class Home extends React.Component {

  render() {

    const people = this.props.people.map( person => {
      return <li>{person}</li>
    });

    return (
      <div>
        <ul>
        {people}
        </ul>
      </div>
    );
  }
}

module.exports = Home;
```
---

### Pairing Exercise:

Start from scratch.

Create an express app.

```
mkdir react-v
cd react-v
npm init
npm install express
npm install express-react-views react react-dom
touch index.js
```

Go to this URL: [https://jsonplaceholder.typicode.com/users](https://jsonplaceholder.typicode.com/users)

Assign that JSON array to a variable `const users = ...`

Implement an express route `/firstuser` - it creates an HTML page with the first user in the array `users[0]`.

This template should display at least 2 data fields for this user.

##### further

Implement an express route `/users` - it creates an HTML page with all of the users in the `const users` variable.

This template should display at least 2 data fields for each user.

##### further
Display the nested pieces of data in the user objects.

You can also put those nested pieces of data in their own components / files.
