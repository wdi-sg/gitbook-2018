# Basic Features

In this section, we will go through some of the basic cool features that Vue provides. In order to understand how does the different features come together or their inner workings, you can check out the official [Vue Guide](https://vuejs.org/v2/guide/) and [documentation](https://vuejs.org/v2/api/).

## Declarative rendering

### Text interpolation

Vue.js allow us to declaratively render data belonging to the Vue application on DOM using the "Mustache" `{{ }}` syntax. This is called **text interpolation**.

```
<div id="myVueApp1">
  {{ message }}{{ appTitle }}
</div>
```

```
var vm1 = new Vue({
  el: '#myVueApp1',
  data: {
    appTitle: "My Vue Application",
    message: "Welcome to "
  }
})
```

Now the data and the DOM element is linked and everything is reactive. What do we mean by reactive? Try opening your Chrome console and execute the following line of code.
```
> vm.appTitle = "My new Vue Application Title"
```
The rendered values `{{ message }}` `{{ appTitle }}` in DOM will be updated accordingly.  

### Directives

Other than **text interpolation**, we can also bind html element attributes to specific data.

```
<div id="myVueApp2" v-bind:title="titleMessage">
  {{ message }}{{ appTitle }}
</div>
```

```
var vm2 = new Vue({
  el: '#myVueApp2',
  data: {
    titleMessage: "You loaded this page on " + new Date(),
    appTitle: "My Vue Application",
    message: "Welcome to "
  }
})
```

The `v-bind` attribute allows you to bind a specific html attribute belonging to DOM element to a data. In this case, we are binding the title element of our `<div id="myVueApp2">` to a data value.

**Without** declarative rendering, the title attribute of `myVueApp2` is static and cannot be changed.
```
<div id="myVueApp2" title="My title">
  {{ message }}{{ appTitle }}
</div>
```
**With** declarative rendering, we can leverage on `v-bind` to make the value of `title` attribute reactive.
```
<div id="myVueApp2" v-bind:title="appTitle">
  {{ message }}{{ appTitle }}
</div>
```

Now that the DOM attribute `title` is bounded to the data `appTitle`, by changing the appTitle value, the `title` attribute of the DOM element will be updated. Try the following in your chrome console and hover your mouse over the element to see the updated value.
```
> vm2.appTitle: "Updated App Title"
```

`v-bind` is what we refer to as **directive** in Vue.js. Directives are prefixed with `v-` to indicate that they are special attributes provided by Vue. Such directive allows you to apply reactive behaviour to the rendered DOM element. Find out more about directives [here](https://vuejs.org/v2/api/#Directives).


## Conditionals and Loops

### If - Else

Vue provides a way to apply simple `if` `else` conditionals in your DOM.

We can use `v-if` to display whether the DOM element is rendered or not by using a truthy or falsey value.
```
<div id="myVueApp3" v-if="seen">
  {{ message }}
</div>
```

```
var vm3 = new Vue({
  el: '#myVueApp3',
  data: {
    seen: true,
    message: "My Vue Application"
  }
})
```

We can also use `else` to render a separate block of DOM elements.
```
<div id="myVueApp3">
  <span v-if="seen"> {{ ifMessage }} </span>
  <span v-else> {{ elseMessage }} </span>
</div>
```
```
var vm3 = new Vue({
  el: '#myVueApp3',
  data: {
    seen: true,
    ifMessage: "My Vue Application",
    elseMessage: "Not My Vue Application"
  }
})
```

Open your console and try changing the data `seen` and see the rendered DOM updated.
```
> vm3.seen = false
```

You can find the detailed Vue Conditionals Guide [here](https://vuejs.org/v2/guide/conditional.html)!

### For loops

Vue.js also provides a similar approach to for loops. We can use `v-for` to iterate an iterable data to render the DOM elements.

```
<div id="myVueApp4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
```

```
var vm4 = new Vue({
  el: '#myVueApp4',
  data: {
    todos: [
      { text: 'Learn JavaScript' },
      { text: 'Learn Vue' },
      { text: 'Build something awesome' }
    ]
  }
})
```

Try the following in your Chrome console and see the DOM updated.

```
> vm4.todos.pop()
> vm4.todos.push({ text: 'So that i can build Something awesome' })
```

Find out more about loops and list rendering [here](https://vuejs.org/v2/guide/list.html)!

## Handling User Input

### Event listeners

In order to make our Vue application interactive, we can use `v-on` directive to attach event listeners that invoke methods declared in our Vue App.
```
<div id="#myVueApp5">
  <p> {{ message }} </p>
  <button v-on:click="reverseMessage"> Reverse the message! </button>
</div>
```
```
var vm5 = new Vue({
  el: '#myVueApp5',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```
A few things are happening in this example.
1. We are rendering the data `message` on the DOM element.
2. We are attaching a click event listener to the `button` element.
3. When the click event of the button is invoked, the method `reverseMessage` is called.
4. `reverseMessage` reverses the `message` data of the Vue App.
5. The rendered DOM is updated accordingly to show the `message` data.

Note that we did not write any DOM manipulation logic and all DOM manipulation is handled by Vue. This setup allows us to focus on the underlying logic of our Vue.js Application.

Find out more about Event Handling [here](https://vuejs.org/v2/guide/events.html)!

### 2-way data binding

Vue also allow us to perform 2-way data binding between form input and the Vue App state by using the `v-model` directive.
```
<div id="myVueApp6">
  <p> {{ message }} </p>
  <input v-model="message">
</div>
```
```
var vm6 = new Vue({
  el: '#myVueApp6',
  data: {
    message: 'Hello Vue.js!'
  }
})
```
`v-model` allow us to bind the value of `input` to the `message` data. If either value of `input` in the DOM or `message` data in the Vue App is changed, it will be reflected on the opposing side.

Find out more about Form Input Binding [here](https://vuejs.org/v2/guide/forms.html)!

## Components

> The component system is another important concept in Vue, because it’s an abstraction that allows us to build large-scale applications composed of small, self-contained, and often reusable components. If we think about it, almost any type of application interface can be abstracted into a tree of components - [Composing with component](https://vuejs.org/v2/guide/index.html#Composing-with-Components)

A Vue component is basically an Vue instance with predefined options. We can register a Vue Component like this:
```
Vue.component('todo-item', {
  template: `<li> A todo-item </li>`
})

var vm7 = new Vue({
  el: '#myVueApp7'
})
```
After registering a Vue Component,
```
<div id="myVueApp7">
  <ol>
    <todo-item><todo-item>
  </ol>
</div>
```

We can also customize the content of components to create more interesting application. We can pass data from the parent-scope to the component by using an option called props.
```
Vue.component('todo-item', {
  props: ['todo'],
  template: `<li> {{ todo.text }} </li>`
})
```
In order to pass the data from the parent-scope to the component, we have to use v-bind to bind the specific property of the component to a data value.
```
<div id="myVueApp7">
  <ol>
    <todo-item v-bind:todo="{ text: "A todo-item" }"><todo-item>
  </ol>
</div>
```
In the example above, we are binding the property `todo` of the component to an object `{ text: "A todo-item" }` which is rendered in the `todo-item` template.

Now that we can pass in data from the parent scope to the components, we can combined with `v-for` to iteratively render a list of `todo-item`.

```
Vue.component('todo-item', {
  props: ['todo'],
  template: `<li> {{ todo.text }} </li>`
})

var vm7 = new Vue({
  el: '#myVueApp7',
  data: {
    todoList:
    [
      { text: 'Vegetables' },
      { text: 'Milk' },
      { text: 'Soda' }
    ]
  }
})
```
```
<div id="myVueApp7">
  <ol>
    <todo-item v-for="item in todoList" v-bind:todo="item"><todo-item>
  </ol>
</div>
```
This is an simple example of breaking an application down into components. In a larger application, it is necessary to break down your application into smaller components to make development and collaboration easier. Find out more about Components [here](https://vuejs.org/v2/guide/components.html)!  

This is an example of how a larger application can look like using components.
```
<div id="app">
  <app-nav></app-nav>
  <app-view>
    <app-sidebar></app-sidebar>
    <app-content></app-content>
  </app-view>
</div>
```
