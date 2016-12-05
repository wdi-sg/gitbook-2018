# Installation Notes for Windows Users (beta)

Some of the programmes that work for Macs may not work for Windows. Here are a list of workarounds / fixes that we've collated through previous courses.

Disclaimer: This page is still in the beta phase. The list of known issues is non-exhaustive and if you discover a fix for a problem that you've faced on Windows which is not covered here, please feel free to let us know.

## ~~Mac Terminal~~ Bash
Download and install Git Bash. Follow instructions here: https://git-for-windows.github.io/

To use Git Bash, you will need to install git first. To know whether you have Git installed on your machine, run ```git --version```. If you get an error, it means you will need to install it by following the instructions here: https://git-scm.com/download/win

## ~~Homebrew~~ Chocolatey

Download chocolatey from https://chocolatey.org/, this will cover a lot of the packages that mac users can brew install

####Setting up SSH Key
You might find your self having to re-authenticate GIT every time you work on your command line. Setup SSH Keys to let Github remember your machine in the future.

* [Github Generating SSH Keys (for Windows)](https://help.github.com/articles/generating-an-ssh-key/#platform-windows)

## ~~Postgres.app~~ PostgresQL

For Windows users, please use this [Postgres client](https://www.postgresql.org/download/windows/) instead of Postgres.app.

The client is quite different from the mac clients (e.g. psequel), so if you encounter issues, please refer to the [postgresql community guide](https://wiki.postgresql.org/wiki/Community_Guide_to_PostgreSQL_GUI_Tools)

##Installing Ruby on Rails

### Option 1: Using Rails Installer

Run the installer from http://railsinstaller.org/en

Make sure to restart your terminal and then run each of these commands. Finally call someone over to validate your installation is correct.

```
rails -v
ruby -v

which ruby
which rails
which bundle
which gem

node -v
npm -v

git --version
psql --version
atom -v

```

### Option 2: c9.io (recommended)
If you encounter issues with installing / running Rails on Windows, we recommend using [c9.io](https://www.c9.io). It's a cloud IDE that is as good as running an IDE on your local machine. You can git commit, git clone, git push - and collaboration with Mac users will be seamless. Here are some instructions for setting up your c9 development environment:

* Sign up for an account with https://www.c9.io (it’s free, even though they ask you for credit card details)
* Create a new project and select Ruby when prompted to choose your language
* To set up your server in postgres (Heroku requires postgres servers), follow this set of instructions: https://github.com/JohnnyBurst/c9_sqlite3_to_pg
* To link the c9.io repo to a github repo, follow these instructions: http://lepidllama.net/blog/how-to-push-an-existing-cloud9-project-to-github/

## npm packages:

* bcrypt:
  * Bcrypt doesn't seem to work well in the windows environment. Use bcrypt-nodejs instead (https://www.npmjs.com/package/bcrypt-nodejs)
