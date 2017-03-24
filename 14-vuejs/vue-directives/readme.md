## In-built directives or attributes
## `v-if` / `v-else`
`v-if` and `v-else` have to be in close proximity with each other.

```html
<div id="app">
  <div v-if="onOff">ON</div>
  <div v-else>OFF</div>
  <button v-on:click="toggleOnOff()">Toggle</button>
</div>
```
Notice how they are all part of the #app element, this means they are all binded to the same instance:
```js
const app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue.js!',
    onOff: true
  },
  methods: {
  // the method called on the button tag would
  // affect the data in the other two divs.
  // Vue syntax !es6
  toggleOnOff() {
  	this.onOff = !this.onOff
    }
  }
})
```
---
## `v-show`
works similar to `v-if` and `v-else`
```html
<!--show if "show" is true-->
<p v-show="show"></p>
```
```js
data: {
  show: true
}
```
---
## `v-model`
```html
<div id="app">
  <div class="search-wrapper">
    <label>Search title:</label>
    <input type="text" v-model="search" placeholder="Search title.."/>
  </div>
</div>
```
the above code is EXACTLY like the one below.
$event.target.value - accesses the DOM
```html
<div id="app>"
  <div class="search-wrapper">
    <label>Search title:</label>
    <input v-bind:value="search" v-on:input="search = $event.target.value" placeholder="Search title">
  </div>
</div>
```
what it's doing is accessing the `const app = new Vue()` instance and going into the `data` object to find the `search` key for it's value.

Refer to the docs for a better explanation [here](https://vuejs.org/v2/guide/forms.html#v-model-with-Components)
```js
const app = new Vue ({
  el: '#app',
  data: {
    search: 'hello'
  }
})
```
---
## `v-for`
```html
<div id="app">
  <div class="wrapper">
  <!--post can be substituted for anything, it's a placeholder for the iteration-->
    <div v-for="post in postList">
      {{ post.author }}
    </div>
  </div>
</div>
```
post in postList is simply saying:
```js
postList.map(function (post, ind) {
  post.author
})
```
---
## `v-bind`
__*binding urls*__
```html
<a v-bind:href="link">Google</a>
```
```js
new Vue ({
  el: "#app"
  data: {
    link: "www.google.com"
  }
})
```
__*Binding CSS classes*__
Defined `.green` and `.pink` in my CSS folder
```html
<div id="app">
  <!--green is the CSS class we bind to v-bind:class-->
  <!--onOff is a dynamic variable-->
  <div class="box" v-bind:class="{ green: onOff, pink: !onOff}"></div>
  <button v-on:click="toggleOnOff()">Toggle</button>
</div>

```
```css
.box { width: 100px;
      height: 100px;
      border: 1px solid rgba(0,0,0,.12); }
.green { background: green; }
.pink { background: hotpink; }
```
Still using the instance method *toggleOnOff()* with the instance object *data.onOff*
```js
const app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue.js!',
    onOff: true
  },
  methods: {
  toggleOnOff() {
  	this.onOff = !this.onOff
	  }
  }
})
```
__*Binding style classes*__
inline-style for elements
```html
<!--color is the CSS selector -->
<!--activeColor is the variable-->
<div v-bind:style="{ color: activeColor }"></div>
```
```js
data: {
  // use string when defining CSS variables
  activeColor: 'red'
}
```
__*Binding href links and img tags*__
refer to `v-for` for better understanding of `post in postList`
```html
<div v-for="post in postList">
  <a v-bind:href="post.link" target="_blank">
    <img v-bind:src="post.img"/>
    <small>posted by: {{ post.author }}</small>
    {{ post.title }}
  </a>
</div>
```
---
## `v-on`
#### Event handling
listening for a click on the submit button and running the `onSubmit` function after. `.prevent` is equal to `event.preventDefault()` in javascript/jQuery
```html
<button type="submit" v-on:click.prevent="onSubmit" class="btn btn-primary">Save product</button>
```
```html
<div id="app">
  <button v-on:click="updateCounter()">Click me</button>
  {{ counter }}
  <!--or you could do in-line expressions-->
  <button v-on:click="counter++">Click me</button>
  {{ counter }}
</div>
```
```js
new Vue ({
  el: "#app"
  data: {
    counter = 0
  },
  methods: {
    updateCounter: function() {
      this.counter++
    }
  }
})
```
---
## `v-text`
#### Update element text content
```html
<div id="app">
  <p v-text="message"></p>
  <!--> same as <-->
  <p>{{message}}</p>
</div>
```

```js
new Vue ({
  el: '#app',
  data: {
    message: 'hello world'
  }
})
```
---

## References
* code-pen - [Dev-coffee](http://codepen.io/AndrewThian/pen/QdeOVa) for the full code
* youtube - [Dev-coffee](https://www.youtube.com/watch?v=VPUdtEf3oXI) for the search filter code along
* youtube - [Traversy tutorial](https://www.youtube.com/watch?v=z6hQqgvGI4Y) for 60 mins crash course on vue
* Chengkoon's starter code - [jsFiddle](https://jsfiddle.net/chengkoon/g4ed1hkq/)
---
