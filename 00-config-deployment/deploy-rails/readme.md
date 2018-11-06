#Deploy Rails on Heroku

##Objectives

* Deploy a Rails app to a production server using Heroku

Deploying a Rails app (like all things in Rails) is simple and magical.

## Postgres
Heroku uses Postgres, so you should too. Or if you don't you should set it to be the DB for use in production.

**Anywhere in Gemfile**

##Prepare for deployment

####Bundle

Once this is done you have to run `bundle` which will automatically update the `Gemfile.lock`, which must be done before you push to Heroku. Be sure to commit your changes before pushing.

##Pushing to Heroku

If you haven't already, download the [Heroku toolbelt](https://toolbelt.heroku.com/), which is a commandline tool used to interact with your heroku instance.

####Create a heroku app

First you need to create a heroku app (if you haven't already). This can be done in the command line inside your project directory.

```rb
heroku create YOUR_APP_NAME_HERE
```

You can test that this worked by running `git remote -v` and you should see a new remote for heroku. If not try again (**pay attention** to any error messages).

####Push it... p-p-p-push it real good

Once that is done we are ready to push our site to heroku. First, run `git status` to make sure you've added and commited all changes then we just push to the newly added heroku remote.

```
git push heroku master
```

(this will take a while to complete)

##Production Database Setup

You can run any command on heroku by using `heroku run` followed by the command you want to execute. This will work for any command you'd normally run on your local development environment. This means that we can run `rake db:migrate` on the server.

**migrate the database**

```
heroku run rake db:migrate
```

You can also run `db:reset` or `db:seed` if you need to reset your database.

**WARNING**
Once you've deployed, don't edit with your migration files. You'll end up with different DBs on your local and production servers and it'll be very difficult to debug. Instead, you should be a well behaved developer and add new migrations for any DB changes.

##Configure environment variables

If your app depends on any environment variables (if you have a `.env` file) you'll need to set those values on Heroku.

To see the currently set environment values run

```
heroku config
```

To set a value run

```
heroku config:set TACO_KEY=xxxxxxx
```

##Viewing your site

Once the push finishes you can go to your website. If you run `heroku open` it will automatically load your web browser at the appropriate URL.

##Updating

Once everything is set up all you have to do is run `git push heroku master` to push new changes to Heroku (always make sure your changes are commited with `git status`).

##Troubleshooting

**What if something goes wrong?**

From your project directory you can run `heroku logs` to see the rails console output on heroku or `heroku logs --tail` to see a live feed.

You can also type `herkou run rails c` to access the rails console on the production server.
