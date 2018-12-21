# Update / Delete

We aren't adding any new concepts when we add the rest of CRUD / rest of the HTTP verbs, but the process of each verb has many steps that must be completed by the server and client.

### Updating
* (Assume user has already created something that is stored as data )
* User requests the form to edit that thing

* Using RESTful style routes that would be `/pokemon/1/edit`

* That requested form already has each piece of data for that record pre-filled in the form:

```js
'<form method="POST" action="/pokemon/'+pokemon.id+'?_method=PUT">'+
  '<div class="pokemon-attribute">'+
    'id: <input name="id" type="text" value="'+pokemon.id+'"/>'+
    'name: <input name="name" type="text" value="'+pokemon.name+'"/>'+
  '</div>'+
'</form>';
```

```js
app.put("/pokemon/:id", (request, response) => {
  //read the file in and write out to it
});
```

### Deleting
* (Assume user has already created something that is stored as data )
* User requests or already has the form to delete that thing

```js
'<form method="POST" action="/pokemon/'+pokemon.id+'?_method=delete">'+
    '<input name="id" type="hidden" value="'+pokemon.id+'"/>'+
    '<input type="submit" value="delete this"/>'+
'</form>';
```

```js
app.delete("/pokemon/:id", (request, response) => {
  //read the file in and write out to it
});
```

---

### MethodOverride

HTML `<form>`s do no yet support PUT and DELETE requests, so we need a way to circumvent the problem.

The `method-override` npm library is designed to get over this limitation.

You will need to install the method-override package using `npm install method-override` and make sure Express uses it.

**index.js**

```js
// Set up method-override for PUT and DELETE forms
const methodOverride = require('method-override')
app.use(methodOverride('_method'));
```

Then, in your HTML page, you will have to specify `_method=<VERB>`, where VERB can be either PUT or DELETE. Here is an example of PUT.

**edit**

```html
'<form method="POST" action="/'+pokemon.id+'?_method=PUT">'+
  '<div class="pokemon-attribute">'+
    'id: <input name="id" type="text" value="'+pokemon.id+'"/>'+
  '</div>'+
'</form>';
```

For more examples and details, as usual, read the [method-override documentation](https://www.npmjs.com/package/method-override).


### Pairing Exercise

Run the following code:

Create and respond to a PUT request:
```
mkdir updat
cd updat
npm init
npm install express
npm install method-override
touch index.js
```

Add the method override config code:

(it goes after you declare the app variable)

```js
// Set up method-override for PUT and DELETE forms
const methodOverride = require('method-override')
app.use(methodOverride('_method'));
```

Add the get route form that creates the request:
```js

app.get('/editsomething',(request, response) => {

  let html = '<form method="POST" action="/putrequest?_method=delete">'+
      '<input name="id" type="hidden" value="'+pokemon.id+'"/>'+
      '<input type="submit" value="delete this"/>'+
  '</form>';

  response.send(html);
});
```

Add the put method route to your app:
```js
app.put("/putrequest", (request, response) => {
  //read the file in and write out to it
  response.send('yes');
});
```

#### Further
Add in some pretend data to edit:

1. install and use `jsonfile`
2. put some fake data in your `data.json`: `{"name":"susan chan"}`
3. render this data in your form
4. use a put request to change the data stored in the json file

#### Further
Make your pretend data more complicated:
```
{
  "names" : [
    "susan",
    "gabriel",
    "chee kean"
  ]
}
```

1. Create a get route for each thing in the list: `/names/0` will render a page with `susan`
2. Add a link to this page to an edit form page: `<a href="/names/0/edit">edit</a>`
3. This page should render a form with the name prepopulated in the form input
4. Change the action of the form to be specific to this particular name: `<form action="/names/0" ....`
5. Change the put request route to handle the editing of a particular element of the names array.

#### Further
Change the names array to contain an array of objects, instead of an array of strings.

```
{
  "names" : [
    {
      "name" : "susan",
      "height" : "12"
    },
    ...

  ]
}
```
