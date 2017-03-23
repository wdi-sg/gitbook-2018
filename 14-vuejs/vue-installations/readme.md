*Has a lot es6 syntax- I'll usually add a small note next to something if it belongs to es6*
## Installations
#### Standalone
Follow the website [here](https://vuejs.org/v2/guide/installation.html)
#### CDN
Import script tag [here](https://unpkg.com/vue)
```html
<script src="https://unpkg.com/vue@2.1.10/dist/vue.js"></script>

```
#### NPM
NPM is the recommended installation method when building large scale applications with Vue. It pairs nicely with module bundlers such as [Webpack](https://webpack.js.org/) or [Browserify](http://browserify.org/). Vue also provides accompanying tools for authoring Single File Components.
#### CLI

```zsh
# install vue-cli
$ npm install --global vue-cli
# create a new project using the "webpack" template
$ vue init webpack my-project[name]
# install dependencies and go!
$ cd my-project[name]
$ npm install
# webpack compile
$ npm run dev
```

## VUE Components
#### Tags
In VUE you have 3 top-level tags:
template tag for your html
```html
<template>
  <h1 class="header">I'm a vue.js component</h1>
</template>
```
script tag for your javascript
```html
<script>
// export default here is es6 syntax
//
export default {
  data () {
    return {}
}
</script>
```
style tag for your styling (LESS, SASS, CSS modules, are also supported by adding additional attributes)
```html
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.header {
  font-size: 4rem;
}
</style>
```
#### DOM element
When when using a component in a template Vue.js expects the DOM element to be lowercased and dasherized.
*e.g. ManageProducts -> manage-products*
```html
<template>
  <!-- comes from ManageProducts.vue -->
  <manage-products></manage-products>
</template>
```

## VUE-loader

## QUESTIONS

* what is $emit?
* what is handling states?
* what are vue's custom events?
* what are props?
* what are prop validations?
* what is v-bind?
* what is v-model or directive?
