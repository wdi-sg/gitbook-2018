## Controllers

We have been placing all routes into `index.js` when creating a Node/Express app, but this can get cumbersome when dealing with many routes. The solution is to separate routes into separate files and attach the routes to the Express router.

**index.js**

```js
const animalsCtrl = require("./controllers/animals")
app.use("/animals", animalsCtrl);
```

**controllers/animals.js**

```js
const express = require("express");
const router = express.Router();

router.get("/", function(req, res) {

});

module.exports = router;
```

Note that the routes should be defined *relative* to the definition in `app.use`. For example, the route defined above in `controllers/animals.js` will be `http://localhost:3000/animals`.
