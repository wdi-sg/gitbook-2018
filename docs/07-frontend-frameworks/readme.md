# Frontend JS Frameworks & Tooling
Much of your full-stack development to this point has focussed on developing thin-clients and fat-servers. That is to say, that the bulk of your logic exists server-side, often with erb/ejs templates being rendered server-side and simply displayed by the browser, with minimal client-side JavaScript. However, as you become more proficient with APIs and AJAX, you will realize that you can often create much richer user experiences without constantly triggering full page reloads, by using client-side JavaScript.

The problem with having more client-side JavaScript is that is can quickly become messy, inefficient and unmanageable. To solve these problems on the backend we employed MVC frameworks and we can do the same client-side.

There are a number of JS frameworks available, the popular ones include Angular, React, Ember and Vue. The benefits include:
* Enabling us to organize and structure **single page apps** using the popular design patterns like MVC
* Making us more productive when developing web apps because they provides features, such as data binding, that requires less code from the developer

##SPA Architecture

Single Page Applications (SPA) are all the rage today. A misconception is that a SPA has only a single view - this is far from the truth!  The single page aspect of a SPA refers to a single page coming from the server, such as our _index.html_ page.  Once loaded, the SPA changes views by using _client-side_ routing, which loads partial HTML snippets called templates.

![spa_architecture](https://cloud.githubusercontent.com/assets/25366/8970635/896c4cce-35ff-11e5-96b2-ef7e62784764.png)

Client-side routing requires something known as a _router_. All of the major frameworks have a router component.

## Tooling
As our client-side code gets more complicated, we need better organization and development practices. To achieve this, we use a number of different tools to do stuff like automatically refresh the browser or convert our ES6 to ES5. The most common categories are outlined below.

### Transpilers

We've seen ES6 and keen to take advantage of the latest features, however, it is not support by all browsers. One way around this is to write our code in ES6 then use a program to convert it into ES6 code - this is called Transpiling - Babel is the most popular ES6 transpiler.

We've also seen Transpiling in Rails, where we convert CoffeeScript to JavaScript, or SASS to CSS. Another popular option these days is Typescript, which implements the ES6 and ES7 standards plus extras - Flow is very similar to TypeScript.


### Module Managers/Loaders
For a large project, keeping all our logic in one file is not a good idea. One option is to include multiple HTML script tags for each file but a better solution is to keep the include logic inside the Javascript itself, like we do in Node.

The two most common approaches are to either bundle all the files into one before distribution (like the Sprockets/Rails Asset Pipeline) or lazy load them only when we need them. The two of the most popular options here are Webpack (bundler) and System JS(lazy-loading).

With lazy-loading the initial application load will be quicker, whereas with bundling, the initial load may be slower but then no future loading is needed.

### Task Runners
When we want to do stuff like transpile our Typescript and SASS, we need to run a task to do it. As we may end up with lots tasks, we often want to use a program to manage them all for us. The most popular solution here is something called Gulp. Gulp allows us to configure multiple plugins to do various tasks such as transpiling(typescript,sass etc) and minification (uglify is the most popular tool for this). We can then set these up to observe our files as run automatically.

When using Gulp it is common to use Browserify to do our module bundling. Webpack also has the ability to include 'loaders' in the process to perform transformations. So developers typically use either Webpack, or Gulp + (Browserify or SystemJS). Before Gulp, Grunt was very popular and a lot of people still use it.

### Package Managers
We've already used package managers on the backend, such as NPM and gem/bundler, as a way to manage our dependencies. NPM is still a great choice for client-side development, other options include Bower and Yarn.

### Live Reloading
We've seen how we can use Nodemon to relaunch our server when the code changes, what we want is something similar client-side. So when we change our JS or HTML, the browser reloads. If we're using Gulp then we can plugin in Browser-sync. If we're using Webpack then their is a webpack-dev-server. If we're using nothing else we can use an package called lite-server.

### Too much!
Stop, please don't burn this book (and your laptop in the process). There are a lot of different tools available, perhaps more than any other area we've covered on the course but to a large part it is because of person preference. Some people like Gulp, some like grunt; some like Webpack some SystemJS; and some dislike everything so build their own tools.

Ultimately, depending on how you/your team like to work, you'll pick the tooling for your project, set it up once and then forget all about it. So don't overthink, whilst this is one of the more confusing areas, it is also not that important - strong coding fundamentals are much more valuable.
