## Your Disk

We won't have to deal with most of the physical components of the computer when writing our programs, but we should understand about what the disk is and does.

### We haven't learned to store anything yet

The result of our programs isn't capable of storing any data yet- one of the most important features of any computer system. We need to be able to build a way to store things on our server, and manipulate them.

(You may have used `localStorage` in your project, but it's just a hack that works around the official spec of how to build a web application.)

The only official way that we've seen to store data is when you press Command-S on your script and HTML files. Your programs don't have the ability to save anything that the user does.

### We can write node.js programs that store data to our disk.

We will be using the `jsonfile` node package to write plain JSON text to our disk. This will come in handy later when we need to store info in our web applications.

Begin in a new directory

```
mkdir jsonfile
cd jsonfile
```

```
yarn init
```

```
yarn add jsonfile
```

```
touch index.js
```

```
sublime index.js
```

```
const jsonfile = require('jsonfile');
const file = 'data.json'
jsonfile.readFile(file, function(err, obj) {
  console.dir(obj)
})
```

Run the file.
```
node index.js
```

Nothing will happen because we specified a file that doesn't exist.

Create the file.
```
touch data.json
```

Run the file.
```
node index.js
```

Nothing will happen because the file is empty.

Put something in the json file.
```
{
  "bannana":"monkey",
  "coconut":"sloth
}
```
Note you don't need to assign it to a variable.

Run the file.
```
node index.js
```
