####Example of classes in ES6
We can use `constructor` in Vue for organizing our code
```js
class Post {
  // constructor function
  // this => bind to instance
  constructor(title, link, author, img) {
    this.title = title;
    this.link = link;
    this.author = author;
    this.img = img;
  }
}
```
Below, `postList` is using the above `class Post` constructor to organize each value with a key.
```js
const app = new Vue ({
  el: '#app',
  data: {
    search: 'hello',
    postList : [
      new Post(
        'Vue.js', 
        'https://vuejs.org/', 
        'Chris', 
        'https://vuejs.org//images/logo.png'
      )
    ]
  }
)
```