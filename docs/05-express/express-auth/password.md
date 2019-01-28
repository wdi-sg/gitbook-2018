# Password

A password is some piece of knowledge that confirms the identity of a user.

A password system works when person A creates a secret word and gives it to person B.

If person A wants to verify themselves to person B they give the password to person B to verify to person B that they are who they say they are.

The implementation of a password is simple- our app will store the user's password, and when they want to identify themselves to us, they will give their password.

First we need to create a user table with a password column.

```
CREATE TABLE IF NOT EXISTS users (
    id SERIAL PRIMARY KEY,
    name TEXT,
    password TEXT
);
```

Then we need an express app. This express app will allow users to create an account and set a password.


### Register and Login

### Logged In Cookies
We will ask the user to verufy their identity once, at the begining of their session, and after that we can give them a cookie that will tell us they logged in at the beggining of their session.

There are 2 situations that we will have verified the user- when they register and when the log in.

In both cases, let's tell the broswer to store a logged in cookie.

```
response.cookie('loggedin', true);
```
