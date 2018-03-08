# Chrome Dev Tools

### Objectives
*After this lesson, students will be able to:*

- Open and describe the components of the Chrome developer tools
- Use the developer tools to manipulate page elements (HTML/CSS)

### Preparation
*Before this lesson, students should already be able to:*

- Open Chrome
- Write basic HTML
- Write basic JavaScript

## What are Dev Tools? – Intro

Most modern browsers include a set of tools that allow developers to monitor and explore what's going on in a web page. The Chrome Developer Tools, which we often call the "Dev Tools", are a set of debugging tools built into Google Chrome.

We can do a lot of useful things with these tools, but some of the things that are most useful:

- We can view the HTML & CSS as the browser has understood them
- We can watch requests and responses as they are made and received
- We can observe JavaScript being run
- We can debug issues with our code
- We can issue JavaScript commands on a console, or browser command line

Having such a powerful set of browser tools at our disposal is incredibly valuable, and you should get into the habit of using them to their full potential.

## Open the DevTools – Demo

First, let's navigate to [http://generalassemb.ly](http://generalassemb.ly).

Now to access the DevTools, we can press:

- `⌘ + ⌥ + i` to open the DevTools (will open on the last tab you had open)
- `⌘ + ⌥ + j` to open the DevTools on the console tab
- `⌘ + SHIFT + c` to open the DevTools with the inspector

If you forget these commands, you can always go to *View > Developer > Developer Tools*, but try to memorize the keyboard shortcut.

#### DevTools Tabs

Overall, there are eight main tools available in the Developer Tools. You may see people with a few more as you can add custom ones using extensions.

- [**Elements**](https://developer.chrome.com/devtools/docs/dom-and-styles): Editing Styles And The DOM
- [**Resources**](https://developer.chrome.com/devtools/docs/resource-panel): Managing application storage
- [**Network**](https://developer.chrome.com/devtools/docs/network): Evaluating network performance
- **Sources**: A graphical interface to the V8 debugger
- [**Timeline**](https://developer.chrome.com/devtools/docs/timeline): Performance profiling with the Timeline
- [**Profiles**](https://developer.chrome.com/devtools/docs/profiles): Profile the execution time and memory usage of a web app or page.
- **Audits**: The Audit panel can analyze a page as it loads.
- [**Console**](https://developer.chrome.com/devtools/docs/console): The JavaScript Console

We won't use all of these tabs during the course, the key ones we are going to get very familiar with are:

- **Elements**
- **Network**
- **Console**

#### Elements Tab

You can use the Elements panel for a variety of tasks:

- Inspecting the HTML & CSS of a web page
- Testing the site in different layouts
- Live-editing CSS on-the-fly

Let's go ahead and play around with some of these tools.

## Modifying CSS On-the-Fly

For modifying and editing your CSS, the dev tools has made it super easy to quickly test and edit CSS before incorporating back into your application. On [http://generalassemb.ly](http://generalassemb.ly) do the following:

- Press `⌘ + SHIFT + c` to open the inspector view
  - You can also do this by opening the dev tools with any other shortcut, then pressing the magnifying glass
  - Or, try right-clicking on the page and selecting "Inspect Element" at the bottom
- Select the whole body
- Look at the DOM nodes on the left hand side
- Look at the CSS responsible for a rendered element in the browser

When you are sure that you have the body selected, in the CSS live editor, uncheck the CSS property `background-image` and underneath it add:

```
element.style {
  background: red;
}
```

What happened? Well since we have this CSS file locally, we're able to do whatever we want to change it!  We removed the `background-image` and replaced it with a solid red `background-color`.  But notice if you refresh your page, everything is gone!  Your browser sent a new request and got a new response back - the default CSS files and folders associated with the `generalassemb.ly` endpoint.

A few other things to try:

- Notice that as you start typing background, the css properties autocomplete
- After choosing your color, a little colored box will show up (if Chrome recognized the color you typed)
- If you press `SHIFT` and click on the colored box, you can see that the color changes format RGBA, Hex, etc.
- If you click on the colored box, you will also see a color selector.
- If you drag your mouse and hover over the screen, you also get a color selector!

Pretty sweet, huh?

Inside this CSS editor, you can also:

- Copy and paste styles
- Use up/down arrow to increment/decrement values by one
- Use `⌥ + ↑` or `⌥ + ↓` to increment/decrement by 0.1
- Use `SHIFT + ↑` or `SHIFT + ↓`to increment/decrement by 10

Let's try it out - inspect the General Assembly logo in the top left corner and change the values of the margin using the tricks above.

We're just trying to get you comfortable editing and manipulating the DOM on your browser as these will come in handy for every one of your projects.

#### Modifying DOM Elements

Inside the DOM tree view, we can see a representation of the Document Object Model as interpreted by the browser.

We can edit these elements.

- Select the `body` element
- Delete it by pressing the `Delete` button
- Then Undo this by pressing `⌘ + z`

#### Tips

- We can copy & paste elements with `⌘ + c` & `⌘ + v`
- We can edit the raw HTML content by *right-clicking* and choosing "Edit as HTML"

#### Viewing Computed Properties

In the CSS Tab, aside from seeing the CSS properties for any inspected element, we can also see a visual representation of the box model along with the computed values. Since CSS loads styles sequentially, it's possible for a style in one stylesheet to be overwritten by another stylesheet that was loaded after it.

The *Computed* tab lets us see the styles for any page or element **exactly as the browser has interpreted all of the CSS styles collectively**.

#### Network Tab

The Network panel records information about each network operation in your application, including detailed timing data, HTTP request and response headers, cookies, WebSocket data, and more.

- Refresh the page and have a look at the requests being made by the page
  - Notice that the status of a lot of the resources are **304** (not modified)
- Select 'Disable Cache' so the requests are processed as new each time we ask for a resource from the server
  - Refresh the page, and you should see that everything is now **200** (ok)

#### Filtering

By default, the network Tab shows all requests being made. However, you can filter these requests by:

- All
- XHR
- Script
- Style
- Images
- Media
- Fonts
- Documents
- WebSockets
- Other

You can also search through these requests, which can be useful.

#### Sourcing Images

If you hover over an image in the network tab, you can right click and `Copy as cURL`

If you paste your clipboard in the terminal, it will paste the curl command to download the file. Pretty cool.

## Console Tab
Lastly, let's have a look at the console tab (practice the shortcut – `⌘ + ⌥ + j`).

The JavaScript Console provides two primary functions for developers testing web pages and applications. It is a place to:

- Log diagnostic information in the development process using the [Console API](https://developer.chrome.com/devtools/docs/console-api)
- A shell prompt which can be used to interact with the document and DevTools.

When we write JavaScript that we intend to be processed in a browser, we can use commands like `console.log()` to log values from our Javascript straight to this tab, as the code executes. This is immensely helpful when we're trying to figure out if certain values are being retrieved or passed between functions. We'll use this feature a lot in the coming weeks.

#### Console Shell

The console shell also allows us to execute Javascript and interact with the current DOM using Javascript, just like we would from a JavaScript file that we load with the page.

Let's try this:

```
> 1 + 1
< 2

> var a = 1;
< undefined

> a
< 1
```

## Independent Practice

Take a look at this screenshot:

![screenshot](https://i.imgur.com/5ruzGhc.png)

Using the Chrome dev tools, try to recreate this screenshot from [Google.com](http://www.google.com).

This is meant to be exploratory!  So you'll have to dig through some HTML elements and their CSS to make this work.  For example, you'll have to modify the `float` property of the image in the search bar - something we haven't touched on yet.

The General Assembly large logo can be sourced from the [General Assembly homepage](https://generalassemb.ly/) and the smaller logo, in the search bar, will have to be found through Google.  
Remember, don't refresh the page, otherwise you'll lose all your work modifying the DOM.  

## Conclusion

There are a lot more things that Chrome can do for you. However, we don't have time to look at them right now. Spend some time playing around with the dev tools during your assignment tonight. You will become very familiar with what they can do for you over the course of WDI.  Check out more shortcuts [here](https://developer.chrome.com/devtools/docs/shortcuts).

- What are the shortcuts to open the dev tools?
- Why do you lose all your work, if you modify the DOM, after you refresh the page?
