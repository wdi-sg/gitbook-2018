Previously, we looked at using components and how to pass data between each of them.

So how to apply them?
By using them as one function block!

refer to [codepen](http://codepen.io/anon/pen/wJBZKG?editors=1011) for more information

```html
<div id='root'>
  <input v-model="msg" placeholder="edit me">
<p>Message is: {{ msg }}</p>
</div>
```
```js
console.clear();

new Vue({
	el: '#root',
	data: {
		msg: 'this is my message'
	},

	methods: {},
	// before hook would fire before any data is defined.
  beforeCreate: function(){
    console.log('@beforeCreate: msg is',this.msg,'. This is triggered only once, at  the start of the component lifecycle. Message is undefined because "data" is still dormant')
  },
   mounted: function(){
    console.log('@mounted: msg is',this.msg, '. This is triggered only once, just after the component is mounted onto the DOM. Data is now active')
  },
   updated: function(){
    console.log('updated: msg is',this.msg, '. This is triggered AFTER every update')
  }
});
```

##Extra references
* [reading](https://alligator.io/vuejs/component-lifecycle/)