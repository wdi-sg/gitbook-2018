# Sessions

We are detecting that the user has logged in by looking at the cookie `logged_in` equal to `true`.

This is a big security vulnerability because anyone could set that cookie in their own browser and then appear to be logged in.

We will fix this by createing a cookie value that's encoded.

It works similarly to how we encoded the password in the db.

We'll call this `session`. Session can actually mean several different things, but in our case we'll define it as a hashed cookie.

### Salt
First we need a secret key or string that will only be known to us. This is the thing that will make it so that a random person can't make their own `logged_in` cookie.

We can just come up with some random value. Because of the properties of a hash function it doesn't actually matter that much how long it is, just that it's secret.

```
const SALT = "bananas are delicious";
```

When the user has registered or logged in:

Create a cookie that is a hashed value.
```
let currentSessionCookie = sha256( user_id + 'logged_id' + SALT );

response.cookie('logged_in', currentSessionCookie);
```

When the user makes a request where they are supposed to be logged in:

Verify the cookie by simply hashing some values and then compare.
```
if( sha256( request.cookies["user_id"] + 'logged_in' + SALT ) === request.cookies["logged_id"] ){

  // you know that the user is logged in
}
```

This is only one way to create what's refered to as a session. We'll see later in rails that it works slightly differently.

But: we now have a relatively secure system- we know with a lot of certainty that a logged in user is who they say they are.
