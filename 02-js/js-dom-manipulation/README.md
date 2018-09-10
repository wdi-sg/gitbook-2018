# Intro to DOM

## DOM Manipulation

---

## Objectives
* Select elements from the DOM using selectors

---

## DOM Manipulation with JavaScript

**Review:** What is the DOM?

Open up the [MDN Website](https://developer.mozilla.org/en-US/)

Go to Developer Console. Look at DOM in *Elements*, then look at the DOM in *Console*. The object 'document' represents the DOM in JavaScript. We can change the DOM, i.e. the page, by changing the **document object**.

Review [DOM on MDN](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)

---

Now, inspect a few properties, for example:

```js
document.URL
document.head
document.links (what does it return?)
```

---

How to change the DOM? Select elements and manipulate them.

**Select by tag id:**

```js
var searchForm = document.getElementById("home-q");
```

What's inside?

```js
searchForm.innerHTML;
```

Also, try `textContent` too! (you may also see this as `innerText`, but this is not supported by all browsers)

---

Change styling:

```js
searchForm.style.backgroundColor = "yellow";
searchForm.style.color = "red";
searchForm.style.height = "100px";
```

---

Properties can be a getter and setter. What does this mean?

**Select by class**

```js
var content_div = document.getElementsByClassName("center");
```

**Select by tag name**
```js
var allListElements = document.getElementsByTagName("li")
```

---

**Preferred: select using CSS selectors**

Get elements by tag name or class is very unspecific. You can go after specific CSS selectors, just like you would in stylesheets:

```js
document.querySelectorAll("li");
document.querySelectorAll("li.nav-main-item");
document.querySelectorAll(".center > h1");
```

---

**Accessing and changing element attributes**

There are 2 ways to get and set attributes of a DOM element. You can access the properties directly or use use get/setAttribute methods. It's important that you know both exist, but generally accessing the properties directly is more consistent across browsers.

```js
//set using property
document.querySelector("a").href = ' http://www.google.com';

//get using property
var href = document.querySelector("a").href;

//get using getAttribute method
var href = document.querySelector("a").getAttribute("href");

//set using setAttribute method
document.querySelector("a").setAttribute("href","http://www.google.com")
```
---

## CSS Classes

Acessing, getting, setting CSS classes is slightly different than other properties.

---

## className

First you can directly access the class attribute by using the `className` property of a DOM element. This works fine, but since elements can have multiple classes (separated by spaces) this often leads to needing to do some string parsing.

---

## classList

To solve the problem of not wanting to do string parsing of the `className` property browsers support the `classList` attribute which gives us an array of classes.

Additionally, the classList attribute has some special methods attached to it.

* add - add a class
* remove - removes a class
* contains - checks if an item has a class

---

**Usage**

```js
//list classes
item.classList

//add a class
item.classList.add('my-new-class');

//remove a class
item.classList.remove('my-new-class');

//check if an item has a class (returns true or false)
item.classList.contains('my-new-class');
```
---

## Compare and Contrast

Compare and contrast the following selectors. Why can't we use querySelector/querySelectorAll for everything?

* getElementById
* getElementsByClassName
* getElementsByTagName
* querySelector
* querySelectorAll

---

### Pairing Exercise (15 minutes)

* Open up your browser and go to the MDN Website.
* Repeat all of the exercies up to this point in the console.
- Create a set of styles for a header tag: `<h1>` that you create with javascript. (example: font-size, color, background color)
- Use `setTimeout` to apply those styles 4 seconds later. (add the class to the element)

---
## DOM - Creation and Deletion

So far we've only seen examples of how to change properties of the DOM
and do things like:

- change the `textContent` of an element
- swap an existing image tag with another
- attach click listeners
- add/remove CSS classes
- modify CSS

We haven't seen the true power of DOM manipulation. The power to create and
destroy DOM elements!!

---

## Creating DOM Elements
The DOM allows us to create elements (make and add new HTML elements to our page).
We can create any HTML tag we want. We can make `<a>` tags, `<p>` tags, `<div>`'s.
Really anything.

To create any new element we follow three basic steps:

1. **create** a new element and save it to a variable
2. **modify** any properties of the element
3. **attach** the element to an existing element on the page

Let's look at an example of how to create a new element in JavaScript and add it to a page.

---

### Adding A New Link to the Bottom of a Page

```js
var a = document.createElement("a");
a.href = "http://thefair.com/";
a.textContent = "thefair.com";

document.body.appendChild(a);
```

---


First, we use `document.createElement` and pass a string representing the tag we
want to create. In this case we pass `"a"` to say we want to make a new link anchor
tag. Save the result of the function into a variable.

`document.createElement` returns a new anchor tag. The element saved in our var
has properties like any other anchor tag on the page, except they're all empty
because it was just created. We must manually add `href` and `textContent` to
set the link and display text.

Finally, we add the new anchor tag to the `<body>` with `document.body.appendChild`.
This adds the anchor to the bottom of the page.

---

#### .appendChild()
The `appendChild()` function exists for all HTML elements. It's easy to show how to
add things to the end of the body of the page because body is a property on the
global document variable.

---

In order to append a new element to any other element, simply obtain a reference
to it first. Here's an example showing how to add a new list item to an unordered
list of my favorite movies.

```html
<ul id="my-favorite-movies">
  <li>Rushmore</li>
  <li>Star Trek VI: Undiscovered Country</li>
  <li>Anchorman</li>
</ul>
```

```js
// obtain a reference to where we'll add it
var list = document.getElementById("my-favorite-movies");

// CREATE
var newMovie = document.createElement("li");

// MODIFY
newMovie.textContent = "Pirates of Silicon Valley";

// ATTACH
list.appendChild(newMovie);
```

---

#### .insertBefore()

```
 ____________
|            |
|  [=====]   |
|  [=====]   |
|  [=====]   |
|          <=|==== append
 ------------

```

```
parent node
 ____________
|            |
|  [=====]   |
|          <=|==== insertBefore
|  [=====] <=|==== reference node
|  [=====]   |
|  [=====]   |
|            |
 ------------
```

It's easy to add things to the end of the body, at the bottom of a div, or at the
end of a list. We have to do slightly more work if we want to add a new element
at the beginning or in the middle of an existing element.

---

Use the following syntax:

```js
var insertedNode = parentNode.insertBefore(newNode, referenceNode);
```

* `parentNode` refers to the container (like body, or a div, or a list)
* `newNode` refers to the element we're adding
* `referenceNode` referes to an element that already exists in the `parentNode`

We can obtain a reference to all of the elements attached to a `parentNode`
by accessing the `children` property.

---

Here's what it looks like all together:

```js
var list = document.getElementById("my-favorite-movies");

var newMovie = document.createElement("li");
newMovie.textContent = "Dr. Strangelove";

// get a reference to the first element inside the list
var first = list.children[0];

// use insertBefore to add newMovie just before the first element.
list.insertBefore(newMovie, first);
```

---

Of course, you can choose any of the children of an element as a `referenceNode`
for `insertBefore`. The new element will simply be added in the spot just before
whatever you choose.

---

## Creating Complex DOM Elements
It's possible to build up complex arrangements of HTML elements in JavaScript
and add them to the page.

```html
<div class="chat-message">
  <div class="info">
    <img class="profile-pic" src="netizen42.png" />
    <div class="username">netizen42</div>
    <div class="timestamp">10:34 PM</div>
  </div>
  <div class="message">I'm hacking into the mainframe now. You better be ready.</div>
</div>
```

---

```js
// create a new div for each container div
var chat = document.createElement("div");
var info = document.createElement("div");

// create elements inside the info container
var img = document.createElement("img");
var user = document.createElement("div");
var time = document.createElement("div");

// create the div for the simple chat message
var msg = document.createElement("div");

img.classList.add("profile-pic");
img.src = "netizen42.png";

user.classList.add("username");
user.textContent = "netizen42";

time.classList.add("timestamp");
time.textContent = "10:34 PM";


msg.classList.add("message");
msg.textContent = "I'm hacking into the mainframe now. You better be ready.";

// add the image, the user and the timestamp to the info container
info.appendChild(img);
info.appendChild(user);
info.appendChild(time);

chat.classList.add("chat-message");

// add the info container and the message to the total chat container
chat.appendChild(info);
chat.appendChild(msg);

// finally, attach the entire new structure to some container on the page.
var parent = document.getElementById("some-container");
parent.appendChild(chat);
```

---

## Destroying DOM Elements

It's way easier to remove elements than it is to add them. Use this syntax:

```
// straight up remove an element without remorse.
parent.removeChild(child);

// you may optional save a reference to the element that was removed.
var oldChild = parent.removeChild(child);
```

`.removeChild()` allows you to remove any element you have reference
to. Notice that it returns a reference to the node you removed. You
may choose to save this to a variable or not. If you save a removed
element to a variable then you have access to it and can add it to
somewhere else on the page.

---

From the examples above:

```
info.removeChild(time);
```

---

## Moving Elements

Practice saving the return value of `.removeChild()` and passing it to
`.appendChild()` to move elements from one list to another.

Hook the functionality up to a button so you move one brunch item to the
lunch list every time it is clicked. Prevent any errors from occuring
if there's nothing left in the breakfast list.

```
<button id="brunch">move to brunch</button>

<h1>Breakfast</h1>
<ul id="breakfast-foods">
  <li>Pancakes</li>
  <li>Waffles</li>
  <li>French Toast</li>
</ul>

<h1>Lunch</h1>
<ul id="lunch-foods">
  <li>Hamburger</li>
  <li>Sandwich</li>
</ul>
```

---

### Optimization to `DOMContentLoaded` : `window.onload`

```
document.addEventListener('DOMContentLoaded',function(){
  // all yur code goes here
});
```

```
window.onload = function(){
  // all your code goes here
};
```

---
## [DOM Manipulation Reference](#reference):
- document.createElement("a");
- document.body.appendChild(a);
- element.appendChild(newElement);
- var insertedNode = parentNode.insertBefore(newNode, referenceNode);
- parent.removeChild(child);

## Pairing Exercise:
- Begin with an HTML file with an empty `body` tag. Create and include a `script.js` file.
- Repeat all of the DOM manipulation examples shown.
