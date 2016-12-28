# Kitchen Sink
Write a function that takes a variable and using the [typeof opertor](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/typeof) console logs different messages.
If it is a:
- string: it should just console log it
- number: it should square it and console log  the result
- boolean: it should console log either 'yes' or 'no'
- function: it should invoke the function()
- undefined: it should scold the user of the function for passing in bad data

### Bonus
If it is an array or object, javascript will return it's type as object, you can use the [isArray Method](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/isArray) to check if it is an array and complete the following extensions:
- array: loop through each element and console log it
- object: loop through each key and console log the value there
- null: is also technically an object so you can work out how to check for that

### Super Bonus
In the above where you are given an array/object, rather than console log the result, call the _kitchenSink function_ again with each element in the array passed in as an arguement - this is called Recursion btw, it's considered pretty advanced stuff :-)
