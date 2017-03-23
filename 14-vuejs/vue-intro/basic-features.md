## Basic Features

In this section, we will go through some of the basic cool features that Vue provides. In order to understand how does the different features come together or their inner workings, you can check out the official [Vue Guide](https://vuejs.org/v2/guide/) and [documentation](https://vuejs.org/v2/api/).

### Declarative rendering

#### Text interpolation

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

#### Directives

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

**Without** declarative rendering, the title attribute of `myVueApp2` is static and is not attached to any data.
```
<div id="myVueApp2" title="My title">
  {{ message }}{{ appTitle }}
</div>
```
**With** declarative rendering, we can leverage on `v-bind` to make the `title` attribute reactive.
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


### Conditionals and Loops

Vue provides a way to apply simple `if` `else` conditionals in your DOM.

We can use `v-if` to display whether the DOM element is shown or not by using a truthy or falsey value.
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
