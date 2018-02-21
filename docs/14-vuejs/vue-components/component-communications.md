#Parent-child communication
Passing data down from parent to child

##Props:
A way for parent and child components to communicate with one another.

`props:` applied to the child component would allow it to recognise what data to recieve from the parent.

Parent component -----> child component by appending a html element in your parent component.

We pass data down with the `msg-from-parent` key:
```html
<!--parent component -->
<template>
  <div>
    <p>I am the parent component</p>
    <!--we pass down 'sedun dnes' data with msg-from-parent key-->
    <child-comp msg-from-parent='sedun dnes'></child-comp>
  </div>
</template>

<script>
import Child from './components/Child.vue';

export default {
  components: { 'child-comp' : Child },
  data: function() {
    return {
      msg : 'Hello world!'
    }
  }
}
</script>
```
And in the child component we reference that key with the `props:` property of the vue instance.

We can recieve the data from the parent component and then call a function within the child `methods:` by passing `msgFromParent`(parent data) as a parameter.
```html
<!--child component-->
<template>
  <div>I am the child component
    <p> this is a message from the parent; pls decode it!</p>
    <p> the decoded msg is : {{ decode(msgFromParent) }} </p>
  </div>
</template>

<script>
  export default {
    props: [ 'msgFromParent' ],
    data: function() {
      return {
        anotherMsg: 'Hello Universe!'
      }
    },
    methods: {
      decode: function(str) {
        return str.split('').reverse().join('')
      }
    }
  }
</script>
```
##Slots:
Slots are a very fast and powerful tool that helps 'slot' data from the parent component into the child component WITHOUT defining `props:`

*By naming the slots, we are able to define where in the child html we want to attach them too.*

Here is an example:

```html
<!--parent component-->
<template>
  <div>
    <p>I am the parent component</p>
    <child-comp :msg-from-parent='text' @emitMutateEvent='text = $event'>
      <h1 slot='moan'>im doing this at 1am and Im so tired</h1>
      <h3 slot='groan'>sigghhh</h3>
    </child-comp>
  </div>
</template>

<script>
import Child from './components/Child.vue';

export default {
  components: { 'child-comp' : Child },
  data: function() {
    return {
      msg : 'Hello world!',
      text: 'sedun dnes'
    }
  }
}
</script>
```
`<slot name="hello"></slot>` slot naming is a good way to format your incoming data in your child components

```html
<!--child component-->
<template>
  <div>I am the child component
    <slot name='groan'></slot>
    <p> this is a message from the parent; pls decode it!</p>
    <p> the decoded msg is : {{ decode(msgFromParent) }} </p>
    <p> {{ msgFromParent }} </p>
    <slot name='moan'></slot>
  </div>
</template>

<script>
  export default {
    props: [ 'msgFromParent' ],
    data: function() {
      return {
        anotherMsg: 'Hello Universe!'
      }
    },
    methods: {
      decode: function(str) {
        return str.split('').reverse().join('')
      }
    }
  }
</script>
```

#Child-parent communication

However, communication from child to parent would be slightly more complex. There are three ways of allowing this child-parent-communication:

1. using `v-model` and `v-bind` + `v-on:event="$event.target.value"`
2. using $emit and event listeners in your parent component(vuejs way)
3. passing down callback functions in your :bind to the child component. Which give the child components the ability to call functions defined in the parent(react way)

## `v-model` or `v-bind v-on:yourEvent="$event.target.value"`
By using this method, you are mutating the original data and it's not recommended as vue will throw you an error.

Refer to [Directives](quiver:///notes/98D098BA-D388-49B0-8B77-529CA3E90625) for how to mutate parent data.

---

## `$emit` and event listeners in parent component
We can emit events for the parents to listen to and change the data to be passed down that way.

Here is an example:
```html
<!--parent component-->
<template>
  <div>
    <p>I am the parent component</p>
    <!--v-bind:msg-from-parent='text' is the passing down-->
    <child-comp :msg-from-parent='text' @emitMutateEvent='text = $event'>
      <h1 slot='moan'>im doing this at 1am and Im so tired</h1>
      <h3 slot='groan'>sigghhh</h3>
    </child-comp>
  </div>
</template>

<script>
import Child from './components/Child.vue';

export default {
  components: { 'child-comp' : Child },
  data: function() {
    return {
      msg : 'Hello world!',
      text: 'sedun dnes'
    }
  }
}
</script>
```
Child component below:
```html
<!--child component-->
<template>
  <div>I am the child component
    <slot name='groan'></slot>
    <p> this is a message from the parent; pls decode it!</p>
    <p> the decoded msg is : {{ decode(msgFromParent) }} </p>
    <p> {{ msgFromParent }} </p>
    <slot name='moan'></slot>
    <button @click='mutateMsg'>click to mutate msg</button>
  </div>
</template>

<script>
  export default {
    props: [ 'msgFromParent' ],
    data: function() {
      return {
        anotherMsg: 'Hello Universe!'
      }
    },
    methods: {
      decode: function(str) {
        return str.split('').reverse().join('')
      },
      mutateMsg: function() {
        this.$emit('emitMutateEvent', 'im going to mutate yoouuuu')
      }
    }
  }
</script>
```
__Step by step:__
1. we pass the text to the child by binding the key in the parent. 
```html
<child-comp :msg-from-parent='text' @emitMutateEvent='text = $event'>
```
2. In our child component, we have a function defined in our methods that emit and event with a payload:
```js
mutateMsg: function() {
        this.$emit('emitMutateEvent', 'im going to mutate yoouuuu')
      }
```

3. this in turn allows the parent component to listen out for the custom emitted event, that mutates the text(state) with the payload referencing the `$event` *(alias for `event.target.value`)*:
```html
<child-comp :msg-from-parent='text' @emitMutateEvent='text = $event'>
```
---

## Passing down callback functions by `v-bind` and recieving them in your `props:` (react style)
We can define a function in our parent component and pass it down to your child component via props, which would then allow your child component to call the function in the parent.

Here is an example:
```html
<!--parent component-->
<template>
  <div>
    <p>I am the parent component</p>
    <child-comp :msg-from-parent='text' @emitMutateEvent='text = $event' :mutateFn='mutateMe'></child-comp>
  </div>
</template>

<script>
import Child from './components/Child.vue';

export default {
  components: { 'child-comp' : Child },
  data: function() {
    return {
      msg : 'Hello world!',
      text: 'sedun dnes'
    }
  },
  methods: {
    mutateMe: function() {
      this.text = 'callback mutation react style'
    }
  }
}
</script>
```
Below is the child compoent that are referenced by parent component:
```html
<!--child component-->
<template>
  <div>I am the child component
    <p> this is a message from the parent; pls decode it!</p>
    <p> the decoded msg is : {{ decode(msgFromParent) }} </p>
    <p> {{ msgFromParent }} </p>
    <button @click='mutateMsg'>click to mutate msg</button>
    <button @click='mutateFn()'>click to mutate msg callback style</button>
  </div>
</template>

<script>
  export default {
    props: [ 'msgFromParent', 'mutateFn' ],
    data: function() {
      return {
        anotherMsg: 'Hello Universe!'
      }
    },
    methods: {
      decode: function(str) {
        return str.split('').reverse().join('')
      },
      mutateMsg: function() {
        this.$emit('emitMutateEvent', 'im going to mutate yoouuuu')
      }
    }
  }
</script>
```
__Step by Step:__

1. We pass a custom method called `mutateMe` that calls text in the returned data function.
```js
//parent component
mutateMe: function() {
      this.text = 'callback mutation react style'
    }
```
2. Then we bind the function to mutateFn to send the function down to the child component.
```html
<!--parent component-->
<child-comp :msg-from-parent='text' @emitMutateEvent='text = $event' :mutateFn='mutateMe'></child-comp>
```
3. We recieve the callback function in our child component:
```js
//child component
props: [ 'msgFromParent', 'mutateFn' ],
```

4. Which then we invoke the mutateFn on:click.
```html
<!--child component-->
<button @click='mutateFn()'>click to mutate msg callback style</button>
```
5. Which is a callback to the parent function.
```js
// parent component
mutateMe: function() {
      this.text = 'callback mutation react style'
    }
```

#Sibling-sibling communication
1. passing data from sibling -> parent component -> sibling again.
2. global bus (dax covered)
3. Vuex (didn't cover, include reference link)

##Passing data from sibling to parent
We can employ the previously explained `$emit` and event listeners methods to facilitate the passing of data

Here is an example:

```html
<!--parent component-->
<template>
  <div>
    <p>I am the parent component</p>
    <hr>
    <child-comp :msg-from-parent='text' @emitMutateEvent='text = $event' :mutateFn='mutateMe' :msg-from-sibling='msgFromSibling'></child-comp>
    <hr>
    <sibling-comp @siblingMsgEvent='msgFromSibling = $event'></sibling-comp>
  </div>
</template>

<script>
import Child from './components/Child.vue';
import Sibling from './components/Sibling.vue';

export default {
  components: { 'child-comp' : Child , 'sibling-comp' : Sibling },
  data: function() {
    return {
      msg : 'Hello world!',
      text: 'sedun dnes',
      msgFromSibling: ''
    }
  },
  methods: {
    mutateMe: function() {
      this.text = 'callback mutation react style'
    }
  }
}
</script>
```
```html
<!--child component-->
<template>
  <div>I am the child component
    <p> this is a message from the parent; pls decode it!</p>
    <p> the decoded msg is : {{ decode(msgFromParent) }} </p>
    <p> {{ msgFromParent }} </p>
    <button @click='mutateMsg'>click to mutate msg</button>
    <button @click='mutateFn()'>click to mutate msg callback style</button>
    <p> {{ msgFromSibling }} </p>
  </div>
</template>

<script>
  export default {
    props: [ 'msgFromParent', 'mutateFn', 'msgFromSibling' ],
    data: function() {
      return {
        anotherMsg: 'Hello Universe!'
      }
    },
    methods: {
      decode: function(str) {
        return str.split('').reverse().join('')
      },
      mutateMsg: function() {
        this.$emit('emitMutateEvent', 'im going to mutate yoouuuu')
      }
    }
  }
</script>
```
```html
<!--sibling component-->
<template>
  <div>
    <p>some msg in component</p>
    <button @click='emitSiblingMsg'>click to emit sibling msg</button>
  </div>
</template>

<script>
  export default {
    data: function() {
      return {
      }
    },
    methods: {
      emitSiblingMsg: function() {
        this.$emit('siblingMsgEvent', 'Hello from sibling!!!')
      }
    }
  }
</script>
```
1. we have to import and attach two siblings under this parent div
```js
import Child from './components/Child.vue';
import Sibling from './components/Sibling.vue';
```
2. Like in the previous example of child to parent, we listen out for the emitted event from both the child component and the sibling component:
```js
// child component
mutateMsg: function() {
        this.$emit('emitMutateEvent', 'im going to mutate yoouuuu')
      }
// sibling component
emitSiblingMsg: function() {
        this.$emit('siblingMsgEvent', 'Hello from sibling!!!')
      }
```
3. Which we can pick up from the parent component yet again
```html
<!--parent component-->
 <sibling-comp @siblingMsgEvent='msgFromSibling = $event'></sibling-comp>
```
4. And send it down to the other child component with props!
```html
<!--parent component-->
<child-comp :msg-from-parent='text' @emitMutateEvent='text = $event' :mutateFn='mutateMe' :msg-from-sibling='msgFromSibling'></child-comp>
```
```js
// child component
props: [ 'msgFromParent', 'mutateFn', 'msgFromSibling' ]
```

##Global bus method
We define a global bus instance in our main.js, import it in our various sibling components and use the same `$emit` and event listener methods.

Here is an example:
```html
```
```js
```

##Vuex method
We'll go through it in the Vue router and Vuex portion of the books.

##References
Some references
* [github code along](https://github.com/cavacado/vue-lesson-1)
* [github code along 2](https://github.com/cavacado/vue-lesson-2)
* [vue-js components](https://vuejs.org/v2/guide/components.html#camelCase-vs-kebab-case)