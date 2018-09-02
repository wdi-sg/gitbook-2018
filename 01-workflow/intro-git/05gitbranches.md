
## Review Basic Git Workflow - Intro (5 mins)

Although you've all been using Git and GitHub for over a month, it's still worthwhile to take a look at some of the core ideas of Git.

#### Why Use Version Control?
When you're working on a project, you sometimes want to be able to retrace your steps, or even revert your project to a previous state.  And often (particularly in the workplace) you need a way to effectively collaborate on a single project without stepping on each others' toes. Version control tools address all of these needs.

#### Why Git?
Git, apart from being free and open source, is also in many ways a superior system to many older version control tools (such as Subversion) because it is a "distributed" version control tool. This means that there is no centralized approval structure for making changes to the project; instead, every student who clones the repository has their own complete copy, which they can then edit and change. This makes it much easier to use when working in groups.

>In addition, Git is much better at handling branching and merging, two big topics we'll be covering today.

#### How Does Git Work?
Git works by creating ['snapshots'](https://git-scm.com/book/en/v1/Getting-Started-Git-Basics), which record the current state of a repo. Each snapshot represents the state of the project at some moment in time.

To create a new snapshot, we use `git add` to select (or "stage") a file or files that have changed since our last snapshot, and `git commit` to actually create a new snapshot which includes those changes.

## Structure of a Git Repo with Branching - Demo (15 mins)

Remember, a Git repository can be imagined as a tree of interconnected nodes, each representing a commit/snapshot. Each of these nodes refers back to one (usually) previous node, which represents the state of the repository before that commit was made.

![Git Repo with Two Commits](https://i.imgur.com/pUWMfdy.png)

Each commit also has a unique name (which allows us to identify it) and a commit message (which tells us what changes the commit makes). `master`, above, is a __branch__ : a reference pointing to some commit in the 'tree' of our repository. New commits can only be made at the end of a branch.

![Git Repo with Three Commits](https://i.imgur.com/GFpv8d8.png)

#### Branching

![Branching, Part 1](https://i.imgur.com/OqSxDt2.png)

In the diagram above, alongside `master` there's another reference called `HEAD`. `HEAD` indicates the point on the repository that we're reading from. When we run `git branch`, new branches get added at wherever `HEAD` points. For instance, if we were to run `git branch structure` on the repo above, here's what would happen.

![Branching, Part 2](https://i.imgur.com/XeGw114.png)

In addition to specifying where new branches go, if HEAD is pointing at the end of a branch, it also means that new commits will be added to that branch. If we want to start adding commits to our new `structure` branch instead of our `master` branch, we have to move `HEAD`; this is done using the command `git checkout`. In particular, we want to checkout the `structure` branch, so we would run `git checkout structure`.  Also, we could have created the `structure branch` and switched the head to that branch all in one command with `git checkout -b structure`.

![Branching, Part 3](https://i.imgur.com/PblGpkm.png)

New commits would then be placed onto the `structure` branch:

```bash
git add .
git commit
(using a .gitkeep as a placeholder until I put files there).'
```

![Branching, Part 4](https://i.imgur.com/i1jhpYU.png)
