#Local Git Repo

---

A git repo is represented by a directory, just like any project. All your files will go in a directory. Try creating a directory with the following files:

- project_git
  - readme.md

Once you're done, we can start on the first git command.

---

## `git init` - Creating a local git repo

Inside the folder, you'll want to run the following command.

```
git init
```

This command will initialize a git repo, which actually creates a hidden folder that stores all the changes made to the project.

You'll only need to do this once per project. If you're cloning a git repo, this step is already taken care of for you.

---

## `git status` - Checking your repo status

```
git status
```

Think of `git status` as your dashboard. The command will show you everything you need to know about the current state of your git repo, from untracked files to any outstanding changes that need to be made.

Currently, there are no commits, so we'll want to put files under the management of git. It'll involve two steps, **adding** and **committing**. These steps are very different.

---

## `git add` - Adding files and directories

You can add files individually or the entire directory, including subfolders.

```
git add readme.md
git add .
```

The second command will add everything in the current working directory. But try running `git status` again. While the `git add` command adds these file changes, they're not actually saved. Think of this as the "staging" area of files where we decide what to permanently save and what to discard. Another good way to think of this is in baseball terms. This staging area is the "on-deck circle", getting things ready before batting.

---

## `git commit` - Saving staged files

Let's say you're happy with your work and want to save a version. This is called **committing**:

```
git commit
```

This should open your editor. Save the screen you see and it should put you back into the terminal.

Now, the changes are permanently saved. The file now has a unique version in git and can be recovered if lost. Make sure everything is *clean* by running `git status` again.

---

## Process for making changes

![](https://git-scm.com/figures/18333fig0201-tn.png)
---

#### File status

Try making changes to the `readme.md` file. See what happens when you run:

```
git status
```

---

#### `git diff` - file differences

How do we find out what changed?

```
git diff readme.md
```

---

#### Stage and save

When we're ready to save those changes

```
git add readme.md
git commit
```

---

#### `git checkout` - checkout changes

Or, we can undo changes. If changes have been made before comitting, we can run `checkout` to reset the file back to its most recent commit state.

```
git checkout readme.md
```

---

#### `git log` - review commit history

Note that once you make a commit, you won't be able to unmodify those files. You can see a list of your commits by running:

```
git log
```

---

#### `git rm` - untrack a file

If a file has been added to git and it needs to be deleted, we can run `git rm` and commit the change.

```
git rm <file>
git add -u
git commit
```

---

## Summary

* `git add` files that become part of your program (track)
* `git status <file>` or `.` to see which files changed.
* `git diff` to see exactly what changed (by line)
* `git commit` file changes to save (commit)
* `git checkout` to dicsard local changes (unmodifiy)
* `git rm` to untrack files (remove)

This is the most simple workflow, things get a bit more complex when you start sharing code and manage larger code bases. But this is a good start.

**IMPORTANT NOTE: Avoid creating git repositories inside other git repositories.**

## Git Best Practices

- NEVER use `git add .`
- ALWAYS add files explicitly. If you have multiple files, use full paths to
    refer to each. Example: `git add foo/bar.md baz/qux.js`
- NEVER use `git commit -m "an example commit message"`
- ALWAYS use `git status` before any other command
- NO commit is too small
- NO commit message is too long
- NEVER nest repositories

## Additional Resources

- [Git Commands Cheatsheet](command-reference.md)
- [Learn Version Control with Git](http://www.git-tower.com/learn/git/ebook)
- [Visualizing Git Commands](https://onlywei.github.io/explain-git-with-d3/)
- [Github Git Cheat Sheet](https://services.github.com/on-demand/downloads/github-git-cheat-sheet.pdf)


## Exercise

Create a new repo:

```
cd ~/code
mkdir git-intro
cd git-intro
git init
```
Check the status of the repo you just created:
```
git status
```

Now start creating your file:
```
sublime index.html
```

When sublime opens, paste this text in:

```
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<body>
  <h1>banana</h1>
</body>
</html>
```

Add this new file: `git add index.html`

Check the current status of your repo:
```
git status
```

Commit it: `git commit`

See the log of the file so far: `git log`

Add a second line:
```
<h2>kumquat</h2>
```

See what changes you made:
```
git diff
```

See the file status:
```
git status
```

Commit your changes:
```
git commit
```

Let's make some new changes:
Take out the h1.
Change kumquat to mango.

Commit as above.

Do `git log` to see a record of your changes.

### Further
Add another HTML file to the repo. Make more changes to index.html as you like. Use `diff` `status` and `add` with specific file paths. e.g.: `git diff index.html`

### Further
How do you unstage a file after you add it, but before you commit it? Google for the solution.

### Further
How do you get back an earlier version of your code? Google for the solution.
