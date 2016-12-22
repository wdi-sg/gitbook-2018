# Kopi Order
Write a function that takes a string which represents a customer's Kopi order. Populate an object with the correct ingredients e.g. __"kopi o kosong"__ would return
```js
{
  kopi: 1,
  sugar: 0,
  evaporatedMilk: false,
  condensedMilk: false,
  ice: false,
  cost: 1.50
}
```

As an extension, your object can include an array or instructions for the server by which they can make the drink e.g.
```javascript
{
  ...
  steps: [
    'poor in 1 portion of kopi',
    ...
  ]
}
```
