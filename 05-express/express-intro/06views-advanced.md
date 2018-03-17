# Advanced View and Templates


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
