##References
* code-pen - [Dev-coffee](http://codepen.io/AndrewThian/pen/QdeOVa) for the full code
* youtube - [Dev-coffee](https://www.youtube.com/watch?v=VPUdtEf3oXI) for the search filter code along

##Instance
Creating a VUE instance
Chances are you only need 1 instance in your app as you have many [Vue components](quiver:///notes/8672E5C0-1DAB-4993-BBB1-50707AA43655) to organize your code
```js
const app = new Vue ({
 data: {
  key: value
 }
})
```
---
##Keywords in the Instance
`el` - takes in a string value that references a html element tag.
`data` - takes in an object that are key value pairs.
```js
const app = new Vue ({
  el: '#app',
  // data is in an object here
  // they are key value pairs
  data: {
    welcomeMsg: 'Hello',
    goodbyeMsg: 'Bye'
  }
})
```
`{{ welcomeMsg }}` - are string interpolations! [Here](https://vuejs.org/v2/guide/syntax.html#Raw-HTML) for more information
```html
<div id="app">
  <h1>{{ welcomeMsg }}</h1>
  <h1>{{ goodbyeMsg }}</h1>
</div>
```
---
##Methods in vue instance
`methods` is methods that are accessible in the DOM via the vue instance.
Applying methods in string interpolation:
```js
const app = new Vue ({
  el: '#app',
  // data is in an object here
  // they are key value pairs
  data: {
    welcomeMsg: 'Hello',
    goodbyeMsg: 'Bye'
  },
  methods: {
    printWelcomeMsg: function() {
      return 'Hello!'
    },
    // use this. to bind data to this function
    dummyFunction: function() {
      return this.welcomeMsg
    },
    dummyFunction: function() {
    // you can also bind functions to each to other
      return this.printWelcomeMsg()
  }
})
```
```html
<div id="app">
  <h1>{{ welcomeMsg }}</h1>
  <h1>{{ goodbyeMsg }}</h1>
  <h2>{{ printWelcomeMsg() }}</h2>
</div>
```
---
##Computed in Vue instance
`computed:` would require more memory and introduce caching problems.

Instead of a computed property, we can define the same function as a method instead. For the end result, the two approaches are indeed exactly the same. However, the difference is that computed properties are cached based on their dependencies. A computed property will only re-evaluate when some of its dependencies have changed.

[Vuejs](https://vuejs.org/v2/guide/computed.html#ad) documentation for more detail
```html
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
```
```js
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // a computed getter
    reversedMessage: function () {
      // `this` points to the vm instance
      return this.message.split('').reverse().join('')
    }
  }
})
```