## Git with github `fork`

### 1.Fork the Repo:
Click on the fork button on the github.com repo page. (Ex: https://github.com/wdi-sg/express-reference )

### 2. Once github has copied the repo over into your account, `CLONE` it to your local computer with the HTTPS url

#### 2b. Use normal local git workflow (https://wdi-sg.github.io/gitbook-2018/01-workflow/intro-git/03localgit.html)[https://wdi-sg.github.io/gitbook-2018/01-workflow/intro-git/03localgit.html]
- __change__
- `git status`
- `git diff`
- `git add`
- `git commit -m 'something'`

### 3.`PUSH` your changes to your newly cloned repo

### 4. Submit your work to us:

#### Make a Pull Request before class starts on the next day
#### Click the pull request button on your own forked repo
#### Make sure to fill in how you're feeling about your submission.

### 5. (extra) Get changes from the parent repo.
When you make a fork, it is not that easy to get changes made to the original parent repo. Here are the steps:

1. create a new remote called "upstream" in your local repo. You have to copy the URL of the parent repo HTTPS address.

Use the `git remote add upstream` command.

Example:
```
git remote add upstream https://github.com/wdi-sg/express-reference.git
```

2. Get the changes. Warning: this can sometimes cause conflicts in your repo.
```
git pull upstream master
```
