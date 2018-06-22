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
