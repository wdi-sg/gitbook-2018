## Your Disk

---
![](https://i1.wp.com/techmits.com/wp-content/uploads/2017/11/Basic-Computer-Architecture.jpg?resize=852%2C465&ssl=1)

We won't have to deal with most of the physical components of the computer when writing our programs, but we should understand about what the disk is and does.

---

### We haven't learned to store anything yet

The result of our programs isn't capable of storing any data yet- one of the most important features of any computer system. We need to be able to build a way to store things on our server, and manipulate them.

(You may have used `localStorage` in your project, but it's just a hack that works around the official spec of how to build a web application.)

The only official way that we've seen to store data is when you press Command-S on your script and HTML files. Your programs don't have the ability to save anything that the user does.

---

### We can write node.js programs that store data to our disk.

We will be using the `jsonfile` node package to write plain JSON text to our disk. This will come in handy later when we need to store info in our web applications.

---

Begin in a new directory

```
mkdir jsondisk
cd jsondisk
```

Make a node project
```
npm init
```

Add the library
```
npm install jsonfile
```

Add your js file
```
touch index.js
```

Edit your file with this text:
```
sublime index.js
```

```
const jsonfile = require('jsonfile');

const file = 'data.json'

jsonfile.readFile(file, (err, obj) => {

  console.log(obj)
})
```

---

Run the file.
```
node index.js
```

Nothing will happen because we specified a file that doesn't exist.

Create the file.
```
touch data.json
```

---

Run the file.
```
node index.js
```

Nothing will happen because the file is empty.

Put something in the json file.
```
{
  "bannana":"monkey",
  "coconut":"sloth"
}
```
Note you don't need to assign it to a variable.

Run the file.
```
node index.js
```

---

### Writing to the file:

```
const jsonfile = require('jsonfile');

const file = 'data.json'

const obj = {
  "hello" : "banana",
  "goodbye" : "banana"
};

jsonfile.writeFile(file, obj, (err) => {
  console.log(err)
});
```
---

### Reading *and* Writing to the file:

So far we can read all the contents of a file, and then write all the contents of a file.

In order to *add* something to the file, we need to *read* the content, then when we have all the data in the file assigned to a variable (`obj` in the example below), we can alter it, then write it back out.

```
const jsonfile = require('jsonfile');

const file = 'data.json'

jsonfile.readFile(file, (err, obj) => {

  console.log(obj);
  obj["helloworld"] = "monkey";

  jsonfile.writeFile(file, obj, (err) => {
    console.log(err)
  });
});
```
---

### Pairing Exercise:

- Follow the setup instructions above.

- Create the JSON file in sublime.

- Read the JSON file.

- Check it against the JSON text in the sublime file.

- Write to the json file.

- Check it against the JSON text in the sublime file.

- Write something new to the json file.

- Check it against the JSON text in the sublime file.

- Read and then write back out to the JSON file.

- Check it against the JSON text in the sublime file.

##### Further
Take 2 command line arguments and write them as key and value into the JSON.
