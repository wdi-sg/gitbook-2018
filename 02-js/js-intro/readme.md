# Intro to Pure Javascript
![](from here: https://git.generalassemb.ly/ga-wdi-lessons/js-intro/blob/master/readme.md)


## Learning Objectives

- Describe the role Javascript plays alongside HTML and CSS


## Framing (5 minutes / 0:05)
- These will be the basics of writing the javascript language.
- These concepts are actually the same througout all computer languages.
- we'll be working inside the browser today, but javascript can be run independently.
![](http://jce-il.github.io/ASOSMA/firefox-ios/general.jpg)
![](https://i.imgur.com/Qgz5eFD.png)


## Javascript: The Client-Side Programming Language of the Web (5 Minutes / 0:20)

<strong>What's a programming language? What can it do that a markup language like HTML can't?</strong></summary>

  > It lets us do things! It lets us act on information, manipulate it, display it, pretty much whatever we want.
  >
  > Javascript enables us to do all that in a browser (i.e., client-side) using the tools you learned in the pre-work like data types, loops and functions.


Brief history: Javascript was created in 10 days by [Brendan Eich](https://en.wikipedia.org/wiki/Brendan_Eich) of Netscape/Mozilla.
* The programming language is not related to Java in any way but its name. "Java" is to "Javascript" as "ham" is to "hamster."
* Javascript has since gone through multiple iterations, the latest being ECMA Script 6 (ES6/2015)


### Why is it the dominant programming language of the web?


Barriers to entry for learning Javascript are very low. No additional software required to run it. Just a text editor and a browser.
- You can even run it directly in the browser via its Javascript console
- Ex. hide images on ESPN website

Javascript is supported by all web browsers.
- [Some browsers](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/Browser_support_for_JavaScript_APIs) support more features than others

Javascript continues to evolve.
- One of the biggest additions to the language was AJAX, which allows use to reload parts of a page without refreshing the entire thing. This had huge implications for user experience.

A lot of Javascript frameworks and libraries (e.g., jQuery, React) have emerged that enable us to do so much more with Javascript (and do it quickly).

---

## Setting Up Our Environment (5 minutes / 0:25)

We're going to spend the next few minutes learning how to set up our development environment, or our work area for working with Javascript. As far as development environments go, this is about as simple as it gets! If it feels unfamiliar and cumbersome to set up at first, don't worry -- speed quickly comes with repetition.

---

### Steps

1. [Create files](#filecreate)
2. Save changes to file(s)
    - `⌘ S`
3. [Open files in chrome and open chrome console](#openinchrome)
    - `⌘ TAB` to switch to Chrome, if open
    - If Chrome isn't open, `⌘ [SPACE]` to open Spotlight (Max OS X Finder Speed Search), and then type Chrome and hit enter when the Chrome icon appears

<a name="filecreate"></a>

---

#### Create `index.html` and `script.js` Files

Creating a JS intro directory that contains an `index.html` and `script.js` file. If you already have a `wdi` directory with contains a `sandbox` directory, you can run the below Terminal commands...

```bash
cd ~/wdi/sandbox
touch index.html script.js
```

The `index.html` and `script.js` files should contain the following...

```html
<!-- index.html -->

<!DOCTYPE html>
<html>
  <head>
    <title>This is the Title</title>
  </head>
  <body>
  <script src="script.js"></script>
  </body>
</html>
```

```js
// script.js

console.log("hello world")
```

<a name="openinchrome"></a>

---
## Open Chrome & Dev Tools

Open the javascript console with Command + Option + J (`⌘ + ⌥ + J`)

---

### The Console as a REPL

The "Console" is an example of a REPL, which is a tool for testing and debugging code. REPL is an acronym that stands for “Read, Evaluate, Print, Loop”.

Think of the REPL as being simply like scratch-paper for code. It's a small programming environment that lets us run Javascript code one line at a time.

What does it do?
  1. (**R**)eads our code.
  2. (**E**)valuates expressions.
  3. (**P**)rints the result to the console, if any (some things result in or ***return*** `undefined`).
  4. Then it (**L**)oops back to the beginning, ready to (**R**)ead the next line of code we feed it. It 'listens' for new code.

> In Chrome, `⌘ + ⌥ + i` opens the chrome dev tools. Here you can do a bunch of stuff like inspect elements and see HTML, CSS and scripts the page has loaded. It allows you to access the console which interacts with the JS that the page has loaded. In our case we'll see that interaction with the code below
