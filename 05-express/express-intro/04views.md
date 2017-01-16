# Views and Templates

### Views

First, we cannot keep using `res.send` to send a response. Ultimately, we'll want to send HTML files back to the client. It would be much more efficient to store them in files. Let's make a folder, `/views`, and create an `index.html` page inside.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Testing a View</title>
  </head>
  <body>
    <h1>Hello world!</h1>
  </body>
</html>
```

Let's modify the `index.js` to send this file via `.sendFile`. In order to use this function, we also need to add where `.sendFile` can find the views.

**index.js**
```js
const express = require('express');
const path = require('path');
const app = express();

// this sets a static directory for the views
app.use(express.static(path.join(__dirname, 'views')));

app.get('/', function(req, res) {
  // use sendFile to render the index page
  res.sendFile('index.html');
});

app.listen(3000);
```

### Templating with ejs

The downside to this method is that we are only sending HTML files, and what if we want to customize what's on the page? On the front-end, we could manipulate the page using jQuery. But on the back-end, we can inject values into the HTML using template engines. So we're going to set up a template engine called **ejs** (embedded JavaScript) and use that instead.

We need to do a couple steps to get the template engine working.

First, install `ejs` by running `npm install --save ejs` in the command line.

Then, replace the `app.use` statement with the following statement (ejs assumed we'll be placing all template files into the `/views` folder, so it's optional if adhering to that syntax).

```js
app.set('view engine', 'ejs');
```

Also, rename the .html file to a .ejs file. We'll see that the `.ejs` extension is optional in the route, but necessary in the file's actual name.


### Templating with Variables

Templating with variables means we can pass in an object to the `.render` function and access those variables inside the ejs template.

**index.js**
```js
const express = require('express');
const app = express();

app.set('view engine', 'ejs');

app.get('/', function(req, res) {
  res.render('index', {name: "Sterling Archer"});
});

app.listen(3000);
```

then we need to update our `index.ejs` to use a templating variable.

**index.ejs**
```html
<!DOCTYPE html>
<html>
  <head>
    <title>Testing a View</title>
  </head>
  <body>
    <h1>Hello, <%= name %>!</h1>
  </body>
</html>
```

The JavaScript being embedded is enclosed by the `<% %>` tags. The addition of the `=` sign on the opening tag means that a value will be printed to the screen. We can also use the following signs to tell EJS to parse code in different ways:

* `<%- name %>` will print out the expression without escaping HTML
  * If the name was `"<span>Sterling Archer</span>"`, then the `<span>` elements won't be escaped.
* `<% name %>` will not print out the expression, but it will execute it
  * Handy for `if` statements and loops

This doesn't only apply to primitive variables. We can even include variable declarations and iterators using ejs.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Testing a View</title>
  </head>
  <body>
    <h1>Hello, <%= name %>!</h1>

    <% var obsessions = ['spying', 'sarcasm', 'Kenny Loggins']; %>

    <ul>
    <% obsessions.forEach(function(item) { %>
      <li><%= item %></li>
    <% }); %>
    </ul>
  </body>
</html>
```

### Partials

Partials can be used to modularize views and reduce repetition. A common pattern is to move the header and footer of a page into separate views, or partials, then render them on each page.

#### Example

**partials/header.ejs**
```html
<!DOCTYPE html>
<html>
<head>
  <title>My Site</title>
</head>
<body>
```

**partials/footer.ejs**
```html
</body>
</html>
```

**index.ejs**
```html
<% include ../partials/header.ejs %>

<h1>Welcome to my site!</h1>

<% include ../partials/footer.ejs %>
```


### Layouts

Previously we used partials to create a header and a footer for our website. Adding a header and a footer to every page can be cumbersome. Why should we have to write the same lines at the beginning and end of every page? We shouldn't! There's a better way!

Instead, we can create a layout that has a special place for our page content. We can define a basic page structure made up of our header and footer and have a place in the middle where all our content will go. In order to do this, another module must be installed.

### Example

**Step 1:**

Install `express-ejs-layouts` via npm

```
npm install --save express-ejs-layouts
```

**Step 2:**

Require the module and add it to the app.

```js
const ejsLayouts = require("express-ejs-layouts");
app.use(ejsLayouts);
```

**Step 3:**

In the root of the views folder, add a layout called `layout.ejs`

**layout.ejs**
```html
<!DOCTYPE html>
<html>
<head>
  <title>Page</title>
</head>
<body>
  <%- body %>
</body>
</html>
```

This layout will be used by all pages, and the content will be filled in where the `<%- body %>` tag is placed. `<%- body %>` is a special tag used by `express-ejs-layouts` that cannot be renamed.

Now we can create another page `animals.ejs` and see that it's content is placed in the page. We can create new pages without having to write the include statements for the header and footer.

First we add a simple route to `app.js`:

```js
app.get("/animals", function(req, res) {
  res.render("animals", {title: "Favorite Animals", animals: ["sand crab", "corny joke dog"]})
});
```

And we create a new file `views/animals.ejs`:

```html
<h1><%= title %></h1>
<ul>
  <% animals.forEach(function(animal) { %>
    <li><%= animal %></li>
  <% }) %>
</ul>
```

Add a simple navigation list to the to of the layout page so there's a link to every page from every page:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Page</title>
</head>
<body>
  <ul>
    <li><a href="/">Favorite Foods</a></li>
    <li><a href="/animals">Favorite Animals</a></li>
  </ul>
  <%- body %>
</body>
</html>

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
