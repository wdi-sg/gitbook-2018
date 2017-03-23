Instance Lifecycle Hooks:

Each Vue instance goes through a series of initialization steps when it is created - for example, it needs to set up data observation, compile the template, mount the instance to the DOM, and update the DOM when data changes. Along the way, it will also invoke some lifecycle hooks, which give us the opportunity to execute custom logic.

There are also other hooks which will be called at different stages of the instance’s lifecycle, for example mounted, updated, and destroyed. All lifecycle hooks are called with their this context pointing to the Vue instance invoking it. You may have been wondering where the concept of “controllers” lives in the Vue world and the answer is: there are no controllers. Your custom logic for a component would be split among these lifecycle hooks.

(Courtesy of https://vuejs.org/.)
![image](https://vuejs.org/images/lifecycle.png)

Creation(Initialization):

Creation hooks allow you to perform actions before your component has even been added to the DOM. Unlike other hooks, creation hooks are also run during server-side rendering.  

A good time to use creation hooks is if you need to set things up in your component both during client rendering and server rendering.

beforeCreate:

```js
<script>
export default {
  beforeCreate() {
    console.log('Nothing gets called before me!')
  }
}
</script>
```

created:

In the created hook, you will have access to reactive data and events. However, templates and virtual DOM have not been mounted or rendered which will happen in the next hook Mounted.

```js

<script>
export default {
  data: () => ({
    property: 'Blank'
  }),

  computed: {
    propertyComputed() {
      console.log('I change when this.property changes.')
      return this.property
    }
  },

  created() {
    this.property = 'Example property update.'
    console.log('propertyComputed will update, as this.property is now reactive.')
  }
}
</script>

```

Mounting(DOM Insertion):
Mounting hooks are the most-used hooks. It allows you to have access to your component immediately before and after the first render. However, they do not run during server-side rendering. Do not use mounting as a form of fetching data. Instead use created especially if you need data during the server-side rendering.

beforeMount:
The beforeMount hook runs right before the initial render happens and after the template or render functions have been compiled.

```js

<script>
export default {
  beforeMount() {
    console.log(`this.$el doesn't exist yet, but it will soon!`)
  }
}
</script>

```
mounted:

During the mounted hook, you will have access to the reactive component, templates, and rendered DOM.

```html

<template>
  <p>I'm text inside the component.</p>
</template>

<script>
export default {
  mounted() {
    console.log(this.$el.textContent) // I'm text inside the component.
  }
}
</script>

```
updating(Diff & Re-render):

Updating hooks are called whenever a reactive property used by your component changes, or something else causes it to re-render. They allow you to hook into the watch-compute-render cycle for your component.

beforeUpdate

The beforeUpdate hook runs after data changes on your component and the update cycle begins, right before the DOM is patched and re-rendered. It allows you to get the new state of any reactive data on your component before it actually gets rendered.

updated

The updated hook runs after data changes on your component and the DOM re-renders.

Previously, we looked at using components and how to pass data between each of them.

beforeDestroy:

beforeDestroy is fired right before teardown. Your component will still be fully present and functional. If you need to cleanup events or reactive subscriptions, beforeDestroy would probably be the time to do it.

By the time you reach the destroyed hook, there’s pretty much nothing left on your component. Everything that was attached to it has been destroyed.





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
