#API Authentication

Whilst many APIs are public, occasionally we'll want to restrict access to our API or to specific content in it, let's look at solutions to achieve both.

_The examples below are Node/Express but the principles apply to Rails too._

## API Key
The simplest solution to secure an API, is to have a single key which must be provided with all API requests.

On our server we check for the presence of the key in each request and compare it against a value stored in our Environment Variables.

```js
app.use((req, res, next) => {
  const apiKey = req.get('API-Key')
  if (!apiKey || apiKey !== process.env.API_KEY) {
    res.status(401).json({error: 'unauthorised'})
  } else {
    next()
  }
})
```

On the Client we send it in the header of each request. The key can either be stored in the source code or manually added to local storage and retrieved by us, this is a easy trick to give a few users admin access.

```js
$.ajax({
    type: 'GET',
    url: 'my/api/route',
    beforeSend: function (xhr) {
        xhr.setRequestHeader('API-Key', window.localStorage['API_KEY'])
    },
    success: function (response) {
        console.log(response.message)
        displayElem.text(response.message)
    },
    error: function(){
        displayElem.text('Cannot Access Secret Information. In your browser developer tools add the API-Key to your local storage and try again.')
    }
})
```

Check out a full example here: https://github.com/wdi-sg/express-api-key-authentication

## User Token
The above approach has major limitations, as we store the token in the client source code, so we probably want a more secure solution. The best approach here is to have a per user token. The user receives this token when they login in and must send it with every request they make to the API. This is similar to how we have worked with sessions but allows our client to be anything that can send a http requests, rather than just our own website.

On our server, we need to generate some form of token to association with the user. We have two popular approaches. The first is to use a package to generate one for us, something like [JWT - check out the guide here](/05-express/express-jwt/readme.html).

Alternatively, we can generate a token ourselves when we hash and save the user password.

```js
UserSchema.pre('save', function (done) {
  const user = this
  if (!user.isModified('password')) return done()

  bcrypt.genSalt(8, (err, salt) => {
    if (err) return done(err)
    bcrypt.hash(user.password, salt, (err, hash) => {
      if (err) return done(err)
      user.password = hash
      crypto.randomBytes(64, (err, buf) => {
        if (err) done(err)
        // generate auth_token
        user.auth_token = base64url(buf)
        done()
      })
    })
  })
})
```

We would then define some middleware, which checked for the token with each request the user makes. This is similar to what Passport does with sessions.

```js
function userLoggedIn (req, res, next) {
  const userEmail = req.get('User-Email')
  const authToken = req.get('Auth-Token')
  if (!userEmail || !authToken) return res.status(401).json({error: 'unauthorised'})

  User.findOne({email: userEmail, auth_token: authToken}, (err, user) => {
    if (err || !user) return res.status(401).json({error: 'unauthorised'})

    req.user = user
    next()
  })
}
```

Check out a full example here: https://github.com/wdi-sg/express-api-user-token-authentication
