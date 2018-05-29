# Git recipes

These are the commands for common tasks. Git can be a bit hard to memorize, since there's quite a few commands,
and what those commands aren't necessarily named what you'd expect them to be named. This is intended be be a place where you can look up a task, and find the git commands needed to perform it.

## Set the git editor to something easier to use than vim

Append this to your .zshrc file:

```
export GIT_EDITOR=pico
```

## Initialize a git repository

```
git init
```

## Remove a git repository

```
rm -rf .git
```

## Unstage files

```
git reset (file)
```

## See the status of staged files

```
git status
```

## Commit all staged files

```
git commit -m "(Add your commit message here)"
```

### Important note!

Committed changes get a bit harder to remove/revert. Be sure you're committing what you want to commit.

## See history of commits

```
git log
```

## Reverse a change

Reversing a commit doesn't really undo it, it just creates another commit that changes the files you committed
back to what they were before the commit. First, you have to find the ID of the commit you'd like to reverse using git log.

Copy that ID, and paste it into this command

```
git revert (Commit ID)
```

## Destroy all changes up to a certain point

It's possible to completely remove all records of changes back to a given commit. It's not a good idea
to do this on a regular basis, but rarely, it becomes necessary.

```
git reset (Commit ID) --hard
```

If you try to push to a remote repository, the remote will just think you haven't received the latest commits,
so you have to force the remote to rewrite it's own history

```
git push origin --force
```

## Create a branch

```
git checkout -b (branch name)
```

## Delete a branch

```
git branch -d (branch name)
```

## Merge a branch into master

```
git checkout master
git merge (other branch name)
```

### NOTE:

You can merge a branch into any other branch. Just check out the branch you want to merge into, then merge just like
above.

```
git checkout (branch name)
get merge (other branch name)
```


## <a name="#quick-guide"></a>Cloning another repo

1. On github.com, go to the repo which you'd like to clone (e.g. http://www.github.com/davified/js-functions)

2. Fork the repo by clicking on the 'Fork' button on the top right corner of the page

3. In your terminal, `cd` to a folder where you want to keep this repo (e.g. Desktop/coding/week-1), and run
```
git clone http://www.github.com/YOUR_GITHUB_USERNAME/js-functions
```
4. You'll now have the repo running on your local machine! Awesome! After making changes to the files, run the following commands to commit and push your changes to github:
```
git add -A
git commit -m "your awesome commit message"
git push -u origin master
```
5. Your code is now on your repo on github!

## Creating your own repo

1. On github.com, create a new repo by clicking on the `+` button on the top right corner of the page.

2. [Skip this step if you've already created your folder, html file(s) and js file(s)] If you haven't, you can create your folder and files for your program:
```
mkdir my-awesome-repo
cd my-awesome-repo
touch index.html style.css script.js
```

3. In the terminal, `cd` to the folder which you want to push to github, and run:
```
git init
git remote add origin YOUR_GITHUB_REPO_URL
```

4. Your local repo is now linked to your github repo. To push your code to github, run the message commands as before:
```
git add -A
git commit -m "your awesome commit message"
git push -u origin master
```
5. Done! Remember to run step 4 (add, commit, push) regularly!

## Set remote origin

Having a repository on your computer is great, but what if other people want to contribute? Github is like a social network for git. It allows many people to work on the same repository and make commits. You have to link your local repository to Github in order to enable all this.

```
git remote add origin (Add Github URL here)
```

## List remotes

Did we add the remote correctly? Let's find out!

```
git remote -v
```

## Push to remote

```
git push (remote name) (optional branch name)
```

### NOTE:

It's generally a good idea to specify which branch you're pushing. You may have a lot of local branches that your collaborators don't care about. Also, it takes more time to push every branch you may have, so usually I use

```
git push origin master
```

in order to push only to the master branch.

## Pull from remote

```
git pull (remote name) (optional branch name)
```

## Delete remote branch

If you do push a branch to a remote, you can delete it like this:

```
git push origin :(branch name)
```

It's important to include the semicolon. If I had a branch named "my_branch", I would delete it from the remote like this:

```
git push origin :my_branch
```
