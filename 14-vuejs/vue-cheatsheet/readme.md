###Mouse-over
[MouseOver](http://jsfiddle.net/JrodR87/1cekfnqw/3/) - Mouse over a div and show another!
```html
<div id="demo">
    <p v-show="active">Show</p>
    <div v-on="mouseover: mouseOver">Hover over me!</div>
</div>
```
```js
var demo = new Vue({
    el: '#demo',
    data: {
        active: false
    },
    methods: {
        mouseOver: function(){
            this.active = !this.active;   
        }
    }
});
```

###Toggle state between true and false
*refer to [Directives and Attributes](quiver:///notes/98D098BA-D388-49B0-8B77-529CA3E90625) for better understanding of* `onOff`
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
Always remember to add `this.` before your variables or values as Vue needs the reference to it from the DOM.
```html
<div id="app">
  <div v-if="onOff">ON</div>
  <div v-else>OFF</div>
  <!--calling the toggleOnOff() method here-->
  <button v-on:click="toggleOnOff()">Toggle</button>
</div>
```
`toggleOnOff` without `()` would wait for the function to run once
`toggleOnOff` with `()` would have a 'state' then 

###Pretty print JSON data
```html
<pre>
  { $data | json }
</pre>
```

###Binding custom img to div in vue
```html
<div v-bind:style="{'background-image': `url(${userProfiles[key].data.avatar_url})`}"></div>
```

###Installing jQuery globally
1. Include the following in your `webpack.dev.conf.js` file:
```js
new webpack.ProvidePlugin({
      $: 'jquery',
      jquery: 'jquery',
      'window.jQuery': 'jquery',
      jQuery: 'jquery'
    })
```
2. run `npm install jquery --save`

###Using bourbon in assets
1. Create a css file in assets
2. Install `bourbon`
3. Watch the scss file in your assets folder
`sass --watch app.scss:app.css`
4. import into the main.js folder
`import './assets/css/app.css'`

###Using scoped styles
```html
<style scoped>
</style>
```

###Style language
```html
<style lang=scss>
</style>
```