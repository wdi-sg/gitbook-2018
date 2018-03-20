# Views and Templates
---

### Views

We cannot keep using `res.send` to send a response. Ultimately, we'll want to send HTML files back to the client.

We want to have this page's HTML be different for each request. How do we do this??

---

### Templating with Handlebar
<span class="non-slide"></span><span class="non-slide"></span>
<span class="non-slide"></span><span class="non-slide"></span>

If we want to customize what's on the page? We're going to set up a template engine called **[Handlebar](http://handlebarsjs.com/)** and use that instead.

We need to do a couple steps to get the template engine working.

---

First, install [`express-handlebars`](https://github.com/ericf/express-handlebars) by running `yarn add express-handlebars` in the command line.

Then, prepare this directory structure on your `node` project.

```
.
├── app.js
└── views
    ├── home.handlebars

1 directories, 2 files
```

---

Once structure is setup, you can setup the `express` view engine to `handlebar` in this manner.

```javascript
const express = require('express')
const handlebars  = require('express-handlebars')

const app = express();


// this line below, sets a layout look to your express project
app.engine('handlebars', handlebars.create().engine);

// this line sets handlebars to be the default view engine
app.set('view engine', 'handlebars');

app.get('/', (req, res) => {
  // running this will let express to run home.handlebars file in your views folder
  res.render('home')
})
```


Notice that all `.html` file is now a `.handlebars` file.

---

### Expression

Templating with variables means we can pass in an object to the `.render` function and access those variables inside the `handlebars` template. These variables are also called `context`

**index.js**

```js
const express = require('express');
const handlebars  = require('express-handlebars')

const app = express();

app.engine('handlebars', handlebars.create().engine);

// this line sets handlebars to be the default view engine
app.set('view engine', 'handlebars');

app.get('/', (req, res) => {
 , 'handlebars')

app.get('/', (req, res) => {
  // giving home.handlebars file an object/context with `name` as a property
  res.render('home', {name: "Sterling Archer"});
});

app.listen(3000);
```

---

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

<span class="non-slide"></span><span class="non-slide"></span>
<span class="non-slide"></span><span class="non-slide"></span>

#### HTML Escaping
By default, `handlebars` will automatically escape the given `context`. We can also use `{{{ }}}` to tell `handlebars` to **NOT** escape the context given:

```js
// context given are these 
var context = {
  name: "<p>Sterling Archer</p>",
  body: "<p>This is a post about &lt;p&gt; tags</p>"
}

app.get('/', (req, res) => {
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

---

#### Block Expressions

Block expressions allow you to define helpers that will invoke a section of your template with a different context than the current. These block helpers are identified by a `#` preceeding the helper name (e.g. `{{#each}}` or `{{#if}}`) and require a matching pair with a `/` sign (e.g. `{{/each}}` or `{{/if}}`.

---

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

---
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

---

### Pairing Exercise:
Implement one template on your express app
