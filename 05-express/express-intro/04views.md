# Views and Templates

### Views

First, we cannot keep using `res.send` to send a response. Ultimately, we'll want to send HTML files back to the client. It would be much more efficient to store them in files. Let's make a folder, `/public`, and create an `index.html` page inside.

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

// this sets a static directory for 'public' folder
app.use(express.static(path.join(__dirname, 'public')));

app.get('/', function(req, res) {
  // use sendFile to render the index page
  res.sendFile('index.html');
});

app.listen(3000);
```

### Templating with Handlebar

The downside to this method is that we are only sending HTML files, and what if we want to customize what's on the page? On the front-end, we could manipulate the page using jQuery. But on the back-end, we can inject values into the HTML using template engines. So we're going to set up a template engine called **[Handlebar](http://handlebarsjs.com/)** and use that instead.

We need to do a couple steps to get the template engine working.

First, install [`express-handlebars`](https://github.com/ericf/express-handlebars) by running `yarn add express-handlebars` in the command line.

Then, prepare this directory structure on your `node` project.

```
.
├── app.js
└── views
    ├── home.handlebars
    └── layouts
        └── main.handlebars

2 directories, 3 files
```

Once structure is setup, you can setup the `express` view engine to `handlebar` in this manner.

```javascript
const express = require('express')
const exphbs  = require('express-handlebars')

var app = express();

// this line below, sets a layout look to your express project
app.engine('handlebars', exphbs({defaultLayout: 'main'}))
// this line sets handlebars to be the default view engine
app.set('view engine', 'handlebars')

app.get('/', function (req, res) {
  // running this will let express to run home.handlebars file in your views folder
  res.render('home')
})
```

Notice that all `.html` file is now a `.handlebars` file.

### Expression

Templating with variables means we can pass in an object to the `.render` function and access those variables inside the `handlebars` template. These variables are also called `context`

**index.js**

```js
const express = require('express');
const exphbs  = require('express-handlebars')

const app = express();

app.engine('handlebars', exphbs({defaultLayout: 'main'}))
app.set('view engine', 'handlebars')

app.get('/', function(req, res) {
  // giving home.handlebars file an object/context with `name` as a property
  res.render('home', {name: "Sterling Archer"});
});

app.listen(3000);
```

then we need to update our `home.handlebars` to use a templating variable.

**views/home.handlebars**
```html
<!DOCTYPE html>
<html>
  <head>
    <title>Testing a View</title>
  </head>
  <body>
    <h1>Hello, {{ name }}!</h1>
  </body>
</html>
```

The JavaScript being embedded is enclosed by the `{{ }}` tags, this in `handlebars` is also called as an `expression`. 

#### HTML Escaping
By default, `handlebars` will automatically escape the given `context`. We can also use `{{{ }}}` to tell `handlebars` to **NOT** escape the context given:

```js
// context given are these 
var context = {
  name: "<p>Sterling Archer</p>",
  body: "<p>This is a post about &lt;p&gt; tags</p>"
}

app.get('/', function(req, res) {
  res.render('home', context);
});


// in home.handlebars
<body>
  <h1>{{name}}</h1>
  {{{body}}}
</body>
```

Results in:

```html
<body>
  <h1>&lt;p&gt; Sterling Archer &lt;p&gt;</h1>
  <p>This is a post about &lt;p&gt; tags</p>
</body>
```

so:

* `double curly braces` will escape the `expression` (e.g. convert `<>` to `&lt;&gt;`
* `triple curly braces` will **NOT** escape the `expression`


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

    <% var obsessions = var obsessions = ['spying', 'sarcasm', 'Kenny Loggins']; %>

    <ul>
    <% obsessions.forEach(function(item) { %>
      <li><%= item %></li>
    <% }); %>
    </ul>
  </body>
</html>
```

#### Block Expressions

Block expressions allow you to define helpers that will invoke a section of your template with a different context than the current. These block helpers are identified by a `#` preceeding the helper name (e.g. `{{#each}}` or `{{#if}}`) and require a matching pair with a `/` sign (e.g. `{{/each}}` or ``{{/if}}``.

Handlebars offers a variety of built-in helpers such as the `if` conditional and `each` iterator.

##### #each 
Say we have the following `context` given to our `handlebars` template.
```js
var context = {
  people: [
    "Yehuda Katz",
    "Alan Johnson",
    "Charles Jolley"
  ]
}
```
you can iterate through each of the people's name with the `#each` helper:

```handlebars
<ul class="people_list">
  {{#each people}}
    <li>{{this}}</li>
  {{/each}}
</ul>
```
will result in:
```js
<ul class="people_list">
  <li>Yehuda Katz</li>
  <li>Alan Johnson</li>
  <li>Charles Jolley</li>
</ul>
```

##### #if & #unless
`Handlebar` can also conditionally render a block with `#if` and `#unless`. 

Say the context are like so:

```handlebars
var context = {
  maybeYesMaybeNo: Math.floor(Math.random()) // 50/50 chance of truth or false
}

// handlebars
<div>
  {{#if maybeYesMaybeNo }} 
  <h1>YES</h1>
  {{/if}}
  
  {{#unless maybeYesMaybeNo }} 
  <h1>It's a no</h1>
  {{/if}}
</div>
```
with the template above, each `h1` will only be displayed **YES** if `maybeYesMaybeNo` is truthy, and show **NO** if otherwise.

### Layouts

A layout is simply a Handlebars template with a `{{{body}}}` placeholder. Usually it will be an HTML page wrapper into which views will be rendered. 

```js
// this line below, creates a layout look to your express project
app.engine('handlebars', exphbs({defaultLayout: 'main'}))
```

With the given code above, `handlebars` sets the layout file to be at `"views/layouts/main.handlebars"`.

The layout into which a view should be rendered can be overridden per-request by assigning a different value to the `layout` request local. The following will render the "home" view with no layout:

```javascript
app.get('/diff', function (req, res, next) {
    res.render('diff', {layout: false});
});
```

#### Example

In the root of the views folder, create a folder called `layouts.handlebars` and add a layout called `main.handlebars`

**views/layouts/main.handlebars**
```html
<!DOCTYPE html>
<html>
<head>
  <title>Page</title>
</head>
<body>
  {{{ body }}}
</body>
</html>
```

This layout will be used by all pages, and the content will be filled in where the `{{{ body }}}` tag is placed. `{{{ body }}}` is a special tag used by `express-handlebars` that cannot be renamed.

Now we can create another page `animals.handlebars` and see that it's content is placed in the page. We can create new pages without having to write the include statements for the header and footer.

First we add a simple route to `app.js`:

```js
app.get("/animals", function(req, res) {
  res.render("animals", {title: "Favorite Animals", animals: ["sand crab", "corny joke dog"]})
});
```

And we create a new file `views/animals.handlebars`:

```html
<h1><%= title %></h1>
<ul>
  {{#each animals}}
    <li>{{this}}</li>
  {{/each}}
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
  {{{ body }}}
</body>
</html>

```

### Partials

Partials can be used to modularize views and reduce repetition. A common pattern is to move the header and footer of a page into separate views, or partials, then render them on each page.

#### Example

**views/partials/header.handlebars**
```html
<!DOCTYPE html>
<html>
<head>
  <title>My Site</title>
</head>
<body>
```

**views/partials/footer.handlebars**
```html
</body>
</html>
```

**views/index.ejs**
```html
{{ > header }}

<h1>Welcome to my site!</h1>

{{ > footer }}
```
