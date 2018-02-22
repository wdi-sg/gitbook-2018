# Things Template Literals Let You Do
This is part of a series of guides written by students from WDI7 Singapore. Originally written by Sharan and Daniel.

## Multiline Strings
Lets you create multiline strings with a lot less syntax than before.
Useful? Yes.

Desired Output
```
It's close to midnight and something evil's lurking in the dark
Under the moonlight you see a sight that almost stops your heart
You try to scream, but terror takes the sound before you make it
You start to freeze as horror looks you right between the eyes
You're paralyzed
```

ES 5
```js
var thrillerLyrics =
"It's close to midnight and something evil's lurking in the dark\n"
+ "Under the moonlight you see a sight that almost stops your heart\n"
+ "You try to scream, but terror takes the sound before you make it\n"
+ "You start to freeze as horror looks you right between the eyes\n"
+ "You're paralyzed"
```

ES 6
```js
var thrillerLyrics =
`It's close to midnight and something evil's lurking in the dark
Under the moonlight you see a sight that almost stops your heart
You try to scream, but terror takes the sound before you make it
You start to freeze as horror looks you right between the eyes
You're paralyzed`
```

## Less ugly string formatting (aka Expression Interpolation)
${expression} notation allows you to use variable values within a string instead of having to concatenate them separately.
Useful? Yes.

Desired Output
```js
var items = [];
items.push("banana");
items.push("apple");
items.push("pineapple");

var total = 20.8;

"You have 3 item(s) in your basket for a total of $20.80"
```

ES 5
```js
console.log("You have " + items.length + " item(s) in your basket for a total of $" + total);
```

ES 6
```js
console.log(`You have ${items.length} item(s) in your basket for a total of $${total}`);
```

## Create code that is HTML-ready
Useful? Not really... but it's pretty cool!

Desired Output
```
<ul>
  <li>Watermelon</li>
  <li>Apple</li>
  <li>Pear</li>
</ul>
```

ES 5
```js
var es5 = fruits.forEach(function(item, index, arr) {
  console.log('<li>' + item + '</li>');
})
// <li>Watermelon</li>
// <li>Apple</li>
// <li>Pear</li>
```

ES 6
```js
var fruits = ['Watermelon', 'Apple', 'Pear']
var html = `<ul>
  ${fruits.map(function (fruits) {
   return `<li>${fruits}</li>`
 }).join('\n  ')}
 </ul>`
console.log(html);
```

## Raw Strings

The special raw property allows you to access the raw strings as they were entered.
```js
String.raw`Hi\n${8+3}!`
// “Hi\n11!"
```

## Resources
* http://exploringjs.com/es6/ch_template-literals.html
* https://ponyfoo.com/articles/es6-template-strings-in-depth
* https://www.sitepoint.com/understanding-ecmascript-6-template-strings/
