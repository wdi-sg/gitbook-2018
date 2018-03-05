# Intro to Browser Javascript
![](from here: https://git.generalassemb.ly/ga-wdi-lessons/js-intro/blob/master/readme.md)

---

![](http://jce-il.github.io/ASOSMA/firefox-ios/general.jpg)
---
![](https://i.imgur.com/Qgz5eFD.png)
---

## Learning Objectives

- Describe the role Javascript plays alongside HTML and CSS
- Describe the DOM

## Framing (5 minutes / 0:05)

We've dabbled with HTML and CSS. There's a bit of interactivity we can program through CSS but not nearly enough! How can we start to add logic, data-handling and behaviors to our web apps? Enter Javascript.

---

#### If a web application or website were a building...

##### HTML: Structure and Content

HTML would be like the most stripped-down version of that building -- just the structure of the building, the building materials, and some content (maybe unfurnished offices, an empty classroom, a set of not-yet-operational bowling-lanes, etc).

##### CSS: Styling

CSS is responsible for the appearance of the building, adding granite floors, polished doors, wooden railings, etc. CSS styles the content of a website to look like more than just black text on a white background.

##### Javascript: Behavior & Functionality

Javascript might be like the building's elevator systems, ID-scanning & entry systems. Javascript handles interactivity and data.

---

## Think-Pair-Share: Cookie Clicker (10 minutes / 0:15)

> 5 minutes exercise. 5 minutes review.

**Spend 2 minutes** playing with [Cookie Clicker](http://orteil.dashnet.org/cookieclicker/). Think about how the page responds to your actions. What is allowing these behaviors to exist?

**Spend 3 minutes** comparing your individual findings in pairs. With your partner, write down three features that you think are powered by Javascript.

---


### Findings

---

#### Interactivity

Javascript allows us to write code that is executed in response to user interaction
* Clicking the cookie: increment the cookie total
* Liking something on Facebook: increment Like counter
* Submitting a Facebook comment: submit comment, appended to post

---

#### No Refreshes ➡ 👍 User Experience

Cookie Clicker updates the page without requiring a full refresh, making for a smoother user experience
* When I click a cookie, Cookie Clicker is able to update the counter without a hard refresh
* When I comment on a post, Facebook is able to process the new comment and render it without refreshing the entire page

---

# What is the DOM?

* Abstraction layer of what is actually happening on the browser screen ( model of the document )

* Alerts javascript to changes in the state of the HTML (events)

* Allows javascript programs to change the state of the HTML and CSS (document object)

* An "API" or interface to the browser screen from javascript

* Connects the networking part to the javascript part of the browser (so that we can connect that part to the screen)
