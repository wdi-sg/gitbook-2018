# NPM and Package Management with NPM

### Context
When we want to use code *other* people have written for node.js, we can use the system of `require` to do it.


### Objectives
- Explain the use of packages, `require`
- Manage package versions
- Explain dependency versioning
- Explain npm, and its purpose
- Update packages and change node version based on work environment

## Node Modules
If we just have another piece of js we want to use in our node program, we can just download it somewhere and then write a require:

```
const libraryModule = require('./someones-module')
```

Sometimes this will be for using an external API:
```
const twitterClient = require('./twitter-client')

twitterClient.createTweet('bananas are delicious');
```

What if there were a system to download these libraries for our use?

---

## NPM - Modules on the Internet


### Requiring outside code in the browser:

JQuery script include:
```
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
```
Bootstrtap CSS include:
```
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css">
```

### Requiring in node.js

```
const libraryModule = require('./someones-module')
```

*But*: Here are a few problems with this single line of code:

- how do we specify the `someones-module`?
- where does it exist on my computer?
- how does it get there?

### NPM and package.json
![https://upload.wikimedia.org/wikipedia/commons/thumb/d/db/Npm-logo.svg/800px-Npm-logo.svg.png](https://upload.wikimedia.org/wikipedia/commons/thumb/d/db/Npm-logo.svg/800px-Npm-logo.svg.png)

In node, a package is a module that is managed by the NPM system.

The npm *package* repository is an online resource that lists public npm packages that you can download from the commandline.

`package.json` is the file NPM uses to save the list of all packages you've downloaded for this project.

Let's make a folder and download dependencies to this project.

Make a directory for the new project.

```
mkdir new-node-thing
cd new-node-thing
```

Let npm know this is a node project- create a `package.json` file in this project directory.
```
npm init
```

Install the library we want to use.

```
npm install cat-ascii-faces
```

Look inside the `package.json` file:
```
sublime package.json
```

Look inside the npm library storage directory to see the actual library:
```
ls -la node_modules
```

Browse the code:
```
sublime node_modules/cat-ascii-faces/.
```

Some other benefits of this process:

- we can hold this library code *outside* the project repo. -Why commit the code that belongs to someone else? The `npm install` we listed in the `package.json` keeps track of it all for us.
- if we run this code on another computer, that computer knows how to get the library code.


### Package Versions
Because each of these libraries are their own applications, the author of these libraries can and will make changes to these libraries.

NPM keeps track of these changes as *versions*. These are noted in `package.json`

NPM says the version this project installed is `2.0.0`
```
"dependencies": {
  "cat-ascii-faces": "2.0.0"
}
```

You can update a package using the `update` command:

```
npm update browser-sync
```

### NPM and version manegement
Let's say you are the author of an NPM package. You are also free to use other people's code. Your library can include other NPM packages.

This is called a *dependency*.

You then have a situation that one package can include another package can include another package, (which could also concieveably include the original package), on to infinity.

Then each of these packages lists their own versions for each `package.json` file in each library.

You might see where this could get relatively complex.


#### The Problem At Hand
Dealing with dependencies and versions.
![https://cdn-images-1.medium.com/max/2000/1*xVArhwHrhwXoBPWlJTGM4g.png](https://cdn-images-1.medium.com/max/2000/1*xVArhwHrhwXoBPWlJTGM4g.png)



#### Example packages:
[https://github.com/sindresorhus/awesome-nodejs#weird](https://github.com/sindresorhus/awesome-nodejs#weird)

## More package.json

The `package.json` file contains various metadata relevant to your application and most importantly, it gives `npm` information that allows it to identify your app.

Here's an example of a `package.json`:

```
{
  "name" : "underscore",
  "description" : "JavaScript's functional programming helper library.",
  "homepage" : "http://documentcloud.github.com/underscore/",
  "keywords" : ["util", "functional", "server", "client", "browser"],
  "author" : "Jeremy Ashkenas <jeremy@documentcloud.org>",
  "contributors" : [],
  "dependencies" : [],
  "repository" : {"type": "git", "url": "git://github.com/documentcloud/underscore.git"},
  "main" : "underscore.js",
  "version" : "1.1.6"
}
```


Note all the metadata attributes of the file (these are just *some* attributes):

- name: name of the package
- version: version of the package
- description: description of the package
- homepage: homepage of the package
- author: author of the package
- contributors: name of the contributors of the package
- dependencies: list of all the third-party packages (dependencies) the package has installed
- repository: repository type and url of the package
- main: entry point of the package

Only two things are actually required for `npm` to recognize your app: name and version.

As long as you have those two pieces of information, `npm` will be able to locate your app when installing packages into your app's `node_modules` folder.

This file also serves as a documentation of your application for other developers to use. Your description can give anyone a quick idea on the purpose of your package and just by skimming your listed dependencies, a fellow developer can quickly see what your app depends on.

In terms of your application dependencies, when working with a team of developers, it is common practice to put `node_modules/*` in your `.gitignore` file. As long as your dependencies are listed in the package.json, a teammate can run `npm install` after cloning your app and have all the listed dependencies installed locally!


NPM makes the distinction between libraries that are just for developing your app, and library code that will run *inside* your app.

It also makes a distinction for code that will run for your entire computer.


Install something for the project you are currently in
```
npm install ascii-cat-faces
```

Install something for the project that you need only while developing:
```
npm install --save-dev ascii-cat-faces
```

Install something you need for your entire computer.
```
npm install -g ascii-cat-faces
```


## More Version Management

But how do we know what version of a package we are using? We do want to avoid those "breaking changes" mentioned earlier after all. Let's take a look at a dependencies attribute in a package.json file that has been given a value.

```json
...
  "dependencies": {
    "accepts": "~1.2.3",
    "content-disposition": "0.5.0",
    "cookie-signature": "1.0.5"
  }
...
```

Each listed dependency has a specified version associated with it, so that we may know what exactly we're working with. There are various ways a dependency version can be declared, and these version values often come with some interesting characters (`~`, `^`, etc.). Here's a chart to help break things down:

| Version Number | Explanation |
| -------------- | ----------- |
| latest | Takes the latest version possible. Not the safest thing to use. |
| 4, 4.*, 4.x, ~4, ^4 | Any version that starts with 4. Takes the latest. |
| >4.8.5 | Choose any version greater than a specific version. Could break your application. |
| <4.8.5 | Choose any version lower than a specific version. |
| >=4.8.5 | Anything greater than or equal to a specific version. |
| <=4.8.5 | Anything less than or equal to. |
| 4.8.3 - 4.8.5 | Anything within a range of versions. The equivalent of >=4.8.3 and <=4.8.5 |
| ~4.8.5 | Any version “reasonably close to 4.8.5″. This will call use all versions up to, but less than 4.9.0 |
| ~4.8 | Any version that starts with 4.8 |
| ^4.8.5 | Any version “compatible with 4.8.5″. This will call versions up to the next major version like 5.0.0. Could break your application if there are major differences in the next major version. |
| ^1.2 | Any version compatible with 1.2 |

Now when working with a team if you ever encounter a scenario where a package may be behaving differently for different developers, you know to check the dependency versions in the `package.json` files. Also, when updating a package you should check to see if there are any breaking changes mentioned in the version documentation.

## Using .gitignore
Node will install many large files to the `node_modules` folder. We don't want nor need to push these to our GitHub repo! Whoever takes our project can run `npm install` after cloning our repo and run with it. So what can we do?

We can make use of a hidden file called .gitignore - inside which we specify what files and folders we would like Git to not track and hence, not push to GitHub.

Here is a [link](https://github.com/wdi-sg/gitbook-2018/blob/master/.gitignore) to our official class `.gitignore` file. Just copy it and put it in each of your repos with a node_modules file.


#### 2 ways to do gitignore

The first way is to create a new repo on GitHub and choose from the dropdown menu what you'd like to ignore. This can be found just before the "Create repository" button - just choose "Node".

The second, slightly more troublesome way is to create your own gitignore file on your local machine. In your project directory on Terminal:
- run `touch .gitignore`
- open .gitignore on your editor, add `node_modules` in a new line
- run your standard git add and commit
- when you push to your remote GitHub repo, `node_modules` will not be pushed

## Review
Test your understanding of the lesson:

- Explain the term "breaking changes" and its cause.
- What is npm and its purpose?
- What is the purpose of the package.json file?


### pairing exercise

Make a directory for the new project.

```
mkdir new-node-thing
cd new-node-thing
```

Let npm know this is a node project- create a `package.json` file in this project directory.
```
npm init
```

Install the library we want to use.

```
npm install cat-ascii-faces
```

Look inside the `package.json` file:
```
sublime package.json
```

Look inside the npm library storage directory to see the actual library:
```
ls -la node_modules
```

Browse the code:
```
sublime node_modules/cat-ascii-faces/.
```

Create a file that will use the code:
```
touch index.js
```

Put code in `index.js` that runs the library.
```
var cats = require('cat-ascii-faces')

console.log(cats()) // returns a random cat
```

Run your file:
```
node index.js
```

#### further
Pick some other libraries. Install them into your project and run them.

[https://github.com/sindresorhus/awesome-nodejs#weird](https://github.com/sindresorhus/awesome-nodejs#weird)
