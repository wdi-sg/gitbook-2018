#AJAX CRUD

By now, we've seen AJAX in action. In this lesson, we'll use it to request CRUD actions from an API, using GET, POST, PUT and DELETE.

To follow along, you can fork/clone the [Peanuts-api repository on Github](https://github.com/wdi-sg/peanuts-api) or you can hit the button below to deploy a working version to Heroku.

[![Deploy Peanuts](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/wdi-sg/peanuts-api/tree/master)

__Be sure to make a note of your heroku app url__

## GET
Let's try out some GET requests on our peanuts API.

```js
var API_PATH = "https://[your-peanut-api-name].herokuapp.com"

$.get(API_PATH + '/peanuts')
.done(function(response) {
  console.log('success listing peanuts', data)
}).fail(function () {
  console.log('error listing peanuts')
})

```

Remember that this is the same as using the AJAX method and specifying the method.

```js
$.ajax({
    url: API_PATH + '/peanuts',
    type: 'GET'
  }).done(function (data) {
    console.log('success listing peanuts', data)
  }).fail(function () {
    console.log('error listing peanuts')
  })
```

## POST
To create a record, we can just switch to making a POST request and include the data we want to send.

**jQuery / JavaScript**

```js
$.ajax({
  url: API_PATH + '/peanuts',
  type: 'POST',
  data: peanutData
}).done(function (data) {
  console.log('success adding peanut')
}).fail(function () {
  console.log('error adding peanut')
})

```

We can capture this data from a form if we like.

**HTML**

```html
<form id="add-peanut">
  <input type="text" name="name" placeholder="Name" required>
  <input type="text" name="cost" placeholder="Cost" required>
  <button type="submit">Add</button>
</form>
```

When the form is submitted, we want to intercept the result and make an AJAX request to the server. When we get the response, we can update the DOM with the new entry or we could reload the list by listing all peanuts again.

**jQuery / JavaScript**

```js
$('#add-peanut').submit(function (event) {
  event.preventDefault() // stop the browser reloading
  var data = $('#add-peanut').serialize() // extract the data from the form
  $('#add-peanut').trigger('reset') // clear the form
  //TODO. AJAX stuff
})
```

##DELETE

Delete should be used to delete an existing item. A delete request contains no payload (`req.body`) and no query string (`req.query`). The only data is expressed via a URL parameter which matches the item's name (`req.params.name`).

**HTML**

```html
<a href="/peanuts/5" class="delete-link">Delete Peanut</a>
```

Without JavaScript, this would link to `GET /peanuts/5` which would simply display the team at that route. However, we can use JavaScript to override the default behavior and send the request with the DELETE verb.

**jQuery / JavaScript**

```js
$('.delete-link').on ('click', function (e) {
  e.preventDefault();
  let linkElem = $(this);
  let peanutUrl = linkElem.attr('href');
  $.ajax({
    method: 'DELETE',
    url: peanutUrl
  }).done(function (data) {
    console.log(data);
    // TODO. update the DOM
  });
});
```

##PUT

Put should be used to update an existing item, we'll most likely use a form to do this.

**HTML**

```html
<form action="/peanuts/5" class="update-peanut">
  <input type="text" name="name" placeholder="Name" value="Previous Name" required>
  <input type="text" name="cost" placeholder="Cost" value="Previous Cost" required>
  <button type="submit">Update</button>
</form>
```

Without JavaScript this form would submit as a `GET` request. Now we need to override this default behavior to initiate a `PUT` via AJAX.

**jQuery / JavaScript**

```js
$('.update-peanut').on ('submit', function (e) {
  e.preventDefault();
  let formElement = $(this);
  let peanutUrl = formElement.attr('action');
  let peanutData = formElement.serialize();
  $.ajax({
    method: 'PUT',
    url: peanutUrl,
    data: peanutData
  }).done(function (data) {
    console.log(data);
    // TODO. stuff when the PUT action is complete
  });
});
```

## Additional Resources

Check out the example [Peanuts-client repository on Github](https://github.com/wdi-sg/peanuts-client) for a complete example.

* [jQuery AJAX Docs](http://api.jquery.com/jquery.ajax/)
* [Some useful jQuery DOM Manipulation Docs](http://api.jquery.com/prepend/)
