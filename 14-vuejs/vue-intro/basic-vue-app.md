# Creating a basic Vue application

To create a basic vue application we have to create a DOM element to act as the root element for the Vue App.

```
<body>
  ...
  <div id="myVueApp"></div>
  ...
</body>
```

Then we create a new Vue instance and attach the Vue instance to the DOM element we have created using the `el` (element) option.

```
var vm = new Vue({
  el: '#myVueApp'
})
```

Now `#myVueApp` is a Vue Application and it possess all capabilities that Vue.js provides.
