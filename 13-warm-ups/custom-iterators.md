# Custom Iterators
Write as many of the the following functions as you can.

### _forEach
It should take two parameters, an array and a callback function. Inside the function use a for loop to loop through each of the array elements. Invoke the callback function in each loop passing it the element, index and array as arguements.

### _map
It should take two parameters, an array and a callback function. Inside the function use a for loop to loop through each of the array elements. Invoke the callback function in each loop passing it the element, index and array as arguements. Take whatever this function returns and make add it to a new array. At the end of the function return the new array.

### _reduce
It should take three parameters, an array, a callback function and an initial 'sum' value. Inside the function use a for loop to loop through each of the array elements. Invoke the callback function in each loop passing it the sum value and the current element. Take the return value of the callback and save it into the sum value. 

__Extension:__ If no iniital sum value is provided, then pass the first two elements to the callback instead

### _filter
It should take two parameters, an array and a callback function. Inside the function use a for loop to loop through each of the array elements. Invoke the callback function in each loop passing it the element, index and array as arguements. If this function returns true then add this element to a new array. At the end of the function return the new array.

### _every
It should take two parameters, an array and a callback function. Inside the function use a for loop to loop through each of the array elements. Invoke the callback function in each loop passing it the element, index and array as arguements. If all callbacks return true then your function should also return true, else it should return false.
