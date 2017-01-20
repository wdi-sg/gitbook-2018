# Continuous Integration, Code Coverage & Deployment with Codeship

### Local Git -> _git push_ -> Github -> Code Ship -> _run test, test pass_ -> Deploy to Heroku

0. `npm install -g istanbul` to see code coverage
1. Make sure tests are passing
2. In app.js, add `var port = process.env.PORT || 3000`
make sure `app.listen(port)`
at the bottom of app.js, `module.exports = app`
3. In test file, include `var app = require(../app)`
4. In package.json, include
```
"scripts": {
  "start" : "node app.js",
  "dev" : "nodemon app.js",
  "cover" : "istanbul cover _mocha"
}
```
5. in terminal, `npm run cover` to see the test coverage by istanbul
6. Create your heroku app via dashboard
7. Create github repo on Github.
8. On your CLI, `git init` and `git remote add origin your-url`
9. create and configure Code Ship
    - Set up commands:
    ```
    nvm install 6.2.2
    npm install
    npm install -g mocha
    npm install -g istanbul
    npm install -g codeclimate-test-reporter
    ```
    - Test pipelines
    ```
    npm run cover
    CODECLIMATE_REPO_TOKEN if you're using codeclimate (copy from codeclimate)
    ```

10. Push to github and watch the build on Code Ship!
