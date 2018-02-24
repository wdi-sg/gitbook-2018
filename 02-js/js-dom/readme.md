# Intro to Javascript
![](from here: https://git.generalassemb.ly/ga-wdi-lessons/js-intro/blob/master/readme.md)

## Learning Objectives

- Describe the role Javascript plays alongside HTML and CSS
## Framing (5 minutes / 0:05)

We've dabbled with HTML and CSS. There's a bit of interactivity we can program through CSS but not nearly enough! How can we start to add logic, data-handling and behaviors to our web apps? Enter Javascript.

### HTML, CSS and Javascript

HTML, CSS and Javascript are technologies, which serve as the basic components of front-end development. Front-end frameworks and libraries that add layers of abstraction (i.e., the ability to do more with less code) make use of these three technologies.

#### If a web application or website were a building...

##### HTML: Structure and Content

HTML would be like the most stripped-down version of that building -- just the structure of the building, the building materials, and some content (maybe unfurnished offices, an empty classroom, a set of not-yet-operational bowling-lanes, etc).

##### CSS: Styling

CSS is responsible for the appearance of the building, adding granite floors, polished doors, wooden railings, etc. CSS styles the content of a website to look like more than just black text on a white background.

##### Javascript: Behavior & Functionality

Javascript might be like the building's elevator systems, ID-scanning & entry systems. Javascript handles interactivity and data.

## Think-Pair-Share: Cookie Clicker (10 minutes / 0:15)

> 5 minutes exercise. 5 minutes review.

**Spend 2 minutes** playing with [Cookie Clicker](http://orteil.dashnet.org/cookieclicker/). Think about how the page responds to your actions. What is allowing these behaviors to exist?

**Spend 3 minutes** comparing your individual findings in pairs. With your partner, [file an issue](https://github.com/ga-wdi-lessons/js-intro/issues/new) on the lesson repo including three features that you think are powered by Javascript.

### Findings

#### Interactivity

Javascript allows us to write code that is executed in response to user interaction
* Clicking the cookie: increment the cookie total
* Liking something on Facebook: increment Like counter
* Submitting a Facebook comment: submit comment, appended to post

#### No Refreshes ➡ 👍 User Experience

Cookie Clicker updates the page without requiring a full refresh, making for a smoother user experience
* When I click a cookie, Cookie Clicker is able to update the counter without a hard refresh
* When I comment on a post, Facebook is able to process the new comment and render it without refreshing the entire page

#### Communication with Servers

While this isn't the case with Cookie Clicker, Javascript has the ability to communicate with an external server. This means we could...
* Store data associated with some user interaction into a database (e.g., cookie quantity, form data, contents of a post)
* Retrieve information that needs to be displayed on a webpage (e.g., latest cookie quantity, user achievements)

> Cookie Clicker uses the browser's [`localStorage`](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage) you save information

This is not an exhaustive list of Javascript properties, but we'll go over these and more in greater detail later on in the course.

So, to sum up the main three components of front-end web development up in one word each...
* HTML: Structure
* CSS: Styling
* Javascript: Behavior

## Javascript: The Client-Side Programming Language of the Web (5 Minutes / 0:20)

<details>
<summary><strong>What's a programming language? What can it do that a markup language like HTML can't?</strong></summary>

  > It lets us do things! It lets us act on information, manipulate it, display it, pretty much whatever we want.
  >
  > Javascript enables us to do all that in a browser (i.e., client-side) using the tools you learned in the pre-work like data types, loops and functions.

</details>
<br>

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

## Setting Up Our Environment (5 minutes / 0:25)

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

### DOM Manipulation
The first 1/2 of creating dynamic web pages with javascript is moving things around on the screen.

We will be using what might be called an interface or library or API to manipulate what we see in the browser.
![](https://i.imgur.com/Qgz5eFD.png)

### What is the DOM
The DOM is a set of methods and objects created in javascript that represent the current state of the browser. It's an API for interacting with the browser. We can call methods and change the values of objects in javascript in order to affect things in the browser and vice versa. (vice versa is events...)

### How do we use it?
https://wdi-sg.github.io/gitbook/02-js/js-dom-events/
https://wdi-sg.github.io/gitbook/02-js/js-dom-manipulation/


## Exercises

The rest of the lesson consists of guided exercises. The goal behind these is to increase your familiarity with Javascript by analyzing the output of different Javascript expressions.

For each exercise you will be working in pairs.

### Setup

1. Clone down the 
2. In Sublime, open 
