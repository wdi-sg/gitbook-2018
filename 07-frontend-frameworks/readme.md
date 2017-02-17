# Front-end Frameworks
Much of your full-stack development to this point has focussed on developing thin-clients and fat-servers. That is to say, that the bulk of your logic exists server-side, often with erb/ejs templates being rendered server-side and simply displayed by the browser, with minimal client-side JavaScript. However, as you become more proficient with APIs and AJAX, you will realize that you can often create much richer user experiences without constantly triggering full page reloads, by using client-side JavaScript.

The problem with having more client-side JavaScript is that is can quickly become messy, inefficient and unmanageable. To solve these problems on the backend we employed MVC frameworks and we can do the same client-side.

There are a number of JS frameworks available, the popular ones include Angular, React, Ember and Vue. The benefits include:
* Enabling us to organize and structure **single page apps** using the popular design patterns like MVC
* Making us more productive when developing web apps because they provides features, such as data binding, that requires less code from the developer

##SPA Architecture

Single Page Applications (SPA) are all the rage today. A misconception is that a SPA has only a single view - this is far from the truth!  The single page aspect of a SPA refers to a single page coming from the server, such as our _index.html_ page.  Once loaded, the SPA changes views by using _client-side_ routing, which loads partial HTML snippets called templates.

![spa_architecture](https://cloud.githubusercontent.com/assets/25366/8970635/896c4cce-35ff-11e5-96b2-ef7e62784764.png)

Client-side routing requires something known as a _router_. All of the major frameworks have a router component.
