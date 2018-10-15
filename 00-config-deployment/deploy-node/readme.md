# Heroku Deployment with Node

## Objectives
* Understand the purpose of Heroku as a Platform as a Service (PaaS)
* Identify other methods of deployment, and their benefits and drawbacks
* Take a working Node app and deploy it to a server
* Make changes to an existing deployment
* Utilize methods for debugging server errors

## Introduction

When we have finished developing a version of our app, we likely want to put it on the internet for other people to see.

### Localhost

Most of what we've developed so far has just run on our own computers. Both our database and our web server have been on our computer. We've done this because it's much easier to develop locally because we don't actually need an internet connection. However, people can't access it easily unless they are also on our local network.

Options?

#### Buy Another Computer

We could just buy another computer somewhere else and use it to run our applications - or even more than one, if needed, and by the way, a server is a computer. We could connect this other computer to the internet and with a bit of configuration, we could allow people to connect to it using a URL.

However, we'd have to buy and look after this computer, have somewhere to store it and ensure that it was working and always connected to the internet. Also, if someone hit an error when they used our app, we might have to stop and start it? Maybe there is a better way?

#### Use a Cloud Computing Platform

We could also use Amazon Web Services or similar cloud services, which provide the servers needed to host applications via the cloud. While many companies use AWS for deployment (such as Netflix), we are expected to not only deploy our system, but also set up the system architecture for our application. This includes logging in to the remote server, setting up the web server, and managing configuration and databases. While this provides a lot of flexibility for larger applications, there's a large learning curve that leans towards Linux system administration. Luckily, there is an even better way.

#### Abstracting Cloud Computing

Heroku is a cloud-based, Platform as a Service (PaaS). Essentially it's a group of virtual machines that run on Amazon Web Services (EC2) and hosts your application code in the cloud. By using git, you can deploy your code directly to Heroku's machines - they call them "dynos" - and seconds later your changes will be live in production.

To deploy an app to Heroku, it's a fairly straightforward step-by-step process.

First you need to link your machine to your Heroku account - a similar process to what we did with Github.


## Before Deploying
1. Make sure you have an account with heroku: https://www.heroku.com/

2. Make sure you have installed the heroku toolbelt - [https://toolbelt.heroku.com/](https://toolbelt.heroku.com/)

###Install Heroku Toolbelt

This is a command-line tool that allows us to use commands in the terminal, similar to the way that we use git.

Once it is installed, you need to login with your heroku credentials:

```
heroku login
Enter your Heroku credentials.
Email: adam@example.com
Password (typing will be hidden):
Authentication successful.
```

We'll have the ability to create free applications using Heroku, but with limitations. Most of those limitations are related to the size of the databases, as well as uptime for the dynos. For free applications, dynos will "go to sleep" when unused for a period of time. This will lead to slow start times when restarting the dynos.

## Deploy

### package.json
Your package.json file is **crucial** - when you deploy your application, Heroku will check the package.json file for all dependencies so whenever you install anything with npm make sure to use `--save`. You can always check your package.json by deleting your node_modules folder and running npm install, this will only install the dependencies listed in the file, so if you application no longer runs, then you know you're missing something.

Once we deploy our application to Heroku, it will automatically install our dependencies by running npm install. It will then start our application, but how does it know what script to start? The easiest option, is to ensure that we have a _start script_ in our `package.json`. Heroku can then simply run `npm start` to start our application.

```json
  "scripts": {
    "start": "node index.js"
  },
```

It is also best practice to include the version on node you are using. You can do this in a section called engines, for example:
```json
  "engines" : {
    "node" : "^6.6.0"
    }
  },
```

### Environment Variables
Environment variables are values that exist in a computer's current environment. They can affect how a computer runs, or how certain commands are executed. Frequently, we'll have variables that are unique to a particular computer. An example is the PORT variable. When we deploy to Heroku, it will automatically allocate us a PORT and our application must listen on that port.

So in your `index.js` file, where you get your server started, include the port number in your app.listen function. Example:

```js
const PORT = process.env.PORT || 3000;

const server = app.listen(PORT, () => console.log('~~~ Tuning in to the waves of port '+PORT+' ~~~'));
```

Variables should also be used to avoid committing values that are sensitive, for example API keys or database URLs, these are cases where we DO NOT WANT TO COMMIT THE VALUES. These values can vary, but if a malicious user gets ahold of them, they can cause disastrous results, especially if the values access an account that costs money or resources.

### Git Based Deploy
Before you create your app in Heroku, be sure your project is being tracked via a git repository

Next create a Heroku app via the command line

```
heroku create
```

Where `sitename` is the name of your app. This will create a url like: `http://sitename.herokuapp.com`

__Add, and Commit all your data to git__ (you may want to push to Github too).

Then push it to Heroku (it's just another git remote afterall), enter the following command

```
git push heroku master
```

In terminal after you deploy your app, type in `heroku ps:scale web=1`. This will ensure you have at least one dyno(process) running


### Heroku Environment variables
In your javascript code, you might have something like `process.env.GOOGLE_KEY`.

In order to add environment variables to Heroku, you run a command to set it per item in our .env file. For example...

```
heroku config:set S3_KEY=8N029N81 S3_SECRET=9s83109d3+583493190
```

You can also do this via the Heroku Dashboard for you App.

### Heroku Postgresql
The database you are running is just another server running on computer. We are connecting to it from your nodejs server through the network. (That's why it has an address and port.)

On Heroku, each of these servers has its own address, contained in an enviroment variable.

First, we need to add it to our app in the heroku dashboard. Heroku calls these add-ons: [https://elements.heroku.com/addons/heroku-postgresql](https://elements.heroku.com/addons/heroku-postgresql)

Now we need to tell our app where the db server is.

To properly set the configs, we need to follow the instructions set out in the node postgres connection pool library page:
[https://github.com/brianc/node-pg-pool](https://github.com/brianc/node-pg-pool)
```
// inside of db.js

//require the url library
//this comes with node, so no need to yarn add
const url = require('url');

//check to see if we have this heroku environment variable
if( process.env.DATABASE_URL ){

  //we need to take apart the url so we can set the appropriate configs
  
  const params = url.parse(process.env.DATABASE_URL);
  const auth = params.auth.split(':');

  //make the configs object
  var configs = {
    user: auth[0],
    password: auth[1],
    host: params.hostname,
    port: params.port,
    database: params.pathname.split('/')[1],
    ssl: true
  };

}else{

  //otherwise we are on the local network
  var configs = {
      user: 'akira',
      host: '127.0.0.1',
      database: 'pokemons',
      port: 5432
  };
}

//this is the same
const pool = new pg.Pool(configs);

```
#### Working with your cloud Postgres DB
**To play with psql for your running db:**
```
heroku pg:psql
```

**To run a tables.sql file on the heroku db**
```
heroku pg:psql < tables.sql
```
Checkout heroku's postgres documentation: [https://devcenter.heroku.com/articles/heroku-postgresql](https://devcenter.heroku.com/articles/heroku-postgresql)

**Download a copy of your db in the cloud and put it into your local DB:**
[https://devcenter.heroku.com/articles/heroku-postgres-import-export](https://devcenter.heroku.com/articles/heroku-postgres-import-export)


### Heroku Add-ons
Heroku Add-ons are an easy way to install and link applications to setup. For instance, if you're using MongoDB, then you'll want to provision an Mlab add-on. Using Cloudinary for image-uploads, then there is an add-on for that.

Since you'll be doing lots of debugging, you should definitely install Papertrail so that you can easily view and search logs and errors on your App (remember once your code is deployed your error messages won't just popup in your terminal).

```
heroku addons:create papertrail:choklad
```

You can also do this via the Heroku Dashboard for you App.


### Don't forget to commit & push
Anytime you make changes you need to remember to git add, git commit and git push to heroku.


## Review
* App deployment
  * Login to Heroku using the Heroku toolbelt (only do once)
  * Create a Procfile and fetch your port in your app
  * Create a Heroku app
  * Push application to Heroku
* Postgres setup
  * Install Heroku's postgres addon
  * Change the config variables and add a conditional for localhost


#### Pairing Exercise
You can start with this copy of the sql model express app to convert it to a Heroku-ready app: [https://github.com/wdi-sg/heroku-node-example](https://github.com/wdi-sg/heroku-node-example)

## Resources

For all resources Heroku-related, check out the documentation on Heroku's website.

https://devcenter.heroku.com/articles/getting-started-with-nodejs

Some interesting (and free) addons you can add to your app:

* [New Relic](https://elements.heroku.com/addons/newrelic)
  * Application monitoring
* [Papertrail](https://elements.heroku.com/addons/papertrail)
  * Logging management
* [Loader.io](https://elements.heroku.com/addons/loaderio)
  * Load testing for web applications
* [Deploy Hooks](https://elements.heroku.com/addons/deployhooks)
  * Send messages to Slack, Email, or an HTTP endpoint on deployment
* [Heroku Scheduler](https://elements.heroku.com/addons/scheduler)
  * Schedule tasks, similar to `cron`
* [Mailgun](https://elements.heroku.com/addons/mailgun)
  * API for sending emails
* [Cloudinary](https://elements.heroku.com/addons/cloudinary)
  * API for image uploads and delivery
