#Git Workflows

So far, we've been using git to save versions of our work, which is one of git's main purposes. The other main purpose has been ignored until now, and that is team collaboration. There are a number of different ways to collaborate with a group of people using git/Github, some of the most popular are outline below.

## Popular Workflows
#### Centralized Workflow
**How It Works**: The remote repo has one single branch on it, `master`. All collaborators have separate clones of this repo, and you can each work independently on separate things. However, before you push, you need to run `git fetch`/`git pull` (with the `--rebase` flag) to make sure that your master branch isn't out of date.

(+) Very simple

(-) Collaboration is kind of clunky.

#### Feature Branch Workflow

**How It Works**: This workflow is very similar to the 'Centralized' workflow. The biggest difference is that there are branches (which helps to keep feature-related commits isolated), and that instead of pushing changes up directly, collaborators (a) push up changes to a new remote branch rather than master, and (b) submit a pull request to ask for them to be added to the remote repo's `master` branch.

(+) Better isolation than Centralized model, but sharing is still easy. Very flexible.

(-) Sometimes it's too flexible - it doesn't distinguish in any meaningful way between different branches, and that lack of structure can be problematic for larger projects.

#### 'Gitflow' Workflow
**How It Works**: Similar to the Feature Branch workflows, but with more rigidly-defined branches. For example:
- Historical Branches : `master` stores official releases, while `develop` serves as a living 'integration branch' that ties together all the standalone features.
- Release Branches : 'release' branches might exist for any given release, to keep all of those materials together.
- Feature Branches : pretty much the same as in the prior model.
- Maintenance/'Hotfix' Branches : branches used to quickly patch issues with production code.

(+) Highly structured - works well for large projects.

(-) Sometimes overkill for something small.

#### Forked Workflow
**How It Works**: This approach uses multiple remote repos; typically, everyone has their own fork of the 'original' project (the version of the repo that's publicly visible. One collaborator plays the role of 'Integration Manager'. This means that they are responsible for managing the official repository and either accepting or rejecting pull requests as they come in.

(+) One person (the "git master") integrates all changes, so there's consistency.

(-) Can get overwhelming for large projects.

## Group Project
Let's practice this, working in groups of 3-5 people. Each group will adopt one of the workflows introduced above.

###Research
The first step is to study, spend 10 minutes reading up on your allocated workflow and discussing it in your group.

[Different Workflows](https://www.atlassian.com/git/tutorials/comparing-workflows)


###Practice App

Work in your project groups to create a group home page. The homepage should have a main page
showing a picture for each person in the team with their name. Additionally, the practice app
should have a page for each person on the team. Clicking on someone's name or picture should
lead to a page with a short bio for the team member.

Work as a team to decide how the project will be set up. Will you create a simple static HTML page,
or will you create a node app, or will you use Ruby on Rails? Everyone will work on their own
bio pages individually. Each team member should add their own link to their bio page on the home
page. Practice working with your prescribed git workflow to complete the website.

###Presentation
Each group will have 20 minutes to share their workflow with the class. Explain/Demonstrating how it works and highlighting the Strengths & Weaknesses.

## Conclusion

You may come across problems when working with git, such as merge conflicts and changes not appearing where you think they should be. Keep in mind that while you have the tools available to solve these problems, the biggest challenge is to figure out how!

![XKCD Git](http://imgs.xkcd.com/comics/git.png)

Indeed, git may take a lifetime to "master", but the only true way to master git is to use it. Here are some resources if you need help, or would like to learn about advanced tools beyond branching and rebasing.

* https://www.atlassian.com/git/tutorials
* http://pcottle.github.io/learnGitBranching
