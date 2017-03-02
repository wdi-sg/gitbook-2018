##Global Components
Provides organization and encapsulation
Reusable code and can be seperated into .vue files

the html tag `<my-component>` is requiring the `template:` to render the component.

*It's very reminiscent of how ejs-layouts renders the partials through layout.ejs into the DOM.*
```html
<div id="app">
  <my-component></my-component>
</div>
```
```js
// first param is the compnent you want to use
// second param is the object
Vue.component('my-component', {
  template: '<p>{{ welcomeMsg }}</p>'
})
```
`data` in this instance would not work because we are overriding the global data in the Vue instance, we would require data to be function and return your objects:
```js
// first param is the compnent you want to use
// second param is the object
Vue.component('my-component', {
  // wrong syntax
  data: {
    welcomeMsg: 'hello
  },
  template: '<p>{{ welcomeMsg }}</p>'
})
```
Below is an example of how to return an object from the `data: function()`

these are considered global components.
So how do we split them out into various other components?
```js
Vue.component('my-component', {
  // wrong syntax
  data: function() {
    return {
      welcomeMsg: 'hello
    }
  },
  template: '<p>{{ welcomeMsg }}</p>'
})
```
##Local components
Here is an example of local components and how to apply them.

refer to [github repo](https://github.com/cavacado/vue-lesson-1/blob/zl/global-local-end/index.html) for the starter code

Now we have 2 vue instances (`#app`, `#app2`)
```html
<div>
    <div id="app">
      <p>I am app 1</p>
      <my-component></my-component>
      <my-component></my-component>
    </div>
    <hr>
    <div id="app2">
    <!--app2 has no reference to the local component of let = comp thus it would throw an error-->
      <p>I am app 2</p>
      <my-component></my-component>
      <my-component></my-component>
    </div>
  </div>
```
`#app` has the component :
`let comp = { object }` 

that is referenced in the `new Vue({ el: '#app' })`

`'my-component'` is referenced in the html as `<my-component>` above

`comp` is the component we are rendering through with the template: `'<p>....</p>'` defined in `let comp = {... template: '<p>..</p>'}`

```js
<script>
    let comp = {
      data: function() {
        return {
          greeting: 'Welcome to your Vue.js app!'
        }
      },
      template: '<p>this is the greeting:  {{ greeting }}</p>'
    }
    new Vue({
      el: '#app',
      components : { 'my-component' : comp  }
    })
    new Vue({
      el: '#app2'
    })
</script>
```

##References
Some references
* [github code along](https://github.com/cavacado/vue-lesson-1)
* [github code along 2](https://github.com/cavacado/vue-lesson-2)
* [vue-js components](https://vuejs.org/v2/guide/components.html#camelCase-vs-kebab-case)