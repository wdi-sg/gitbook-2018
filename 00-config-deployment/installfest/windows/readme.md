# Installation Notes for Windows Users (beta)

Some of the programmes that work for Macs may not work for Windows. Here are a list of workarounds / fixes that we've collated through previous courses.

Disclaimer: This page is still in the beta phase. The list of known issues is non-exhaustive and if you discover a fix for a problem that you've faced on Windows which is not covered here, please feel free to let us know.

## ~~Mac Terminal~~ Bash
Download and install Git Bash. Follow instructions here: https://git-for-windows.github.io/

To use Git Bash, you will need to install git first. To know whether you have Git installed on your machine, run ```git --version```. If you get an error, it means you will need to install it by following the instructions here: https://git-scm.com/download/win

## Zsh (an alternative shell to Git Bash)

If your mac-user-classmates are happily trodding along with Oh My Zsh, and you'd like to trod along as well, follow the instructions here: https://gingter.org/2016/08/17/install-and-run-zsh-on-windows/ (Note: You will need to install this from your Ubuntu Bash)

## ~~Homebrew~~ Chocolatey

Download chocolatey from https://chocolatey.org/, this will cover a lot of the packages that mac users can brew install

##Node

To install Node, go to https://nodejs.org/en/download/ and follow the installation instructions.

Verify the installation afterwards by running

```
node -v
npm -v
```

The above should display without any errors.

To finish up your installation, run this command to allow for global installations of npm tools.

```
sudo chown -R $USER /usr/local/lib
```

#### GitHub
You might find your self having to re-authenticate GIT every time you work on your command line.
We'll mainly be using HTTPS, so setup a Keychain helper: https://help.github.com/articles/caching-your-github-password-in-git/#platform-windows

For SSH Keys you can cache them too if needed: https://help.github.com/articles/generating-an-ssh-key/#platform-windows

## ~~Postgres.app~~ PostgresQL

For Windows users, please use this [Postgres client](https://www.postgresql.org/download/windows/) instead of Postgres.app.

The client is quite different from the mac clients (e.g. psequel), so if you encounter issues, please refer to the [postgresql community guide](https://wiki.postgresql.org/wiki/Community_Guide_to_PostgreSQL_GUI_Tools)

##Installing Ruby on Rails

### Option 1: Using Rails Installer

Run the installer from http://railsinstaller.org/en

#### PG Gem
If you have issues with the pg gem, you can set it to only be used on production and just use the default SQLite option when you generate your app i.e. no `-d postgres` option.

**Anywhere in Gemfile**

```rb
gem 'pg', group: :production
```

and when you bundle install, skip production
```rb
bundle install --without production
```


### Option 2: c9.io
If you encounter issues with installing / running Rails on Windows, we recommend using [c9.io](https://www.c9.io). It's a cloud IDE that is as good as running an IDE on your local machine. You can git commit, git clone, git push - and collaboration with Mac users will be seamless. Here are some instructions for setting up your c9 development environment:

* Sign up for an account with https://www.c9.io (it’s free, even though they ask you for credit card details)
* Create a new project and select Ruby when prompted to choose your language
* To set up your server in postgres (Heroku requires postgres servers), follow this set of instructions: https://github.com/JohnnyBurst/c9_sqlite3_to_pg
* To link the c9.io repo to a github repo, follow these instructions: http://lepidllama.net/blog/how-to-push-an-existing-cloud9-project-to-github/

## npm packages:

* bcrypt:
  * Bcrypt doesn't seem to work well in the windows environment. Use bcrypt-nodejs instead (https://www.npmjs.com/package/bcrypt-nodejs)


## Verify your installation

Make sure to restart your terminal and then run each of these commands (run only the commands for the programmes that you've installed!). Finally call someone over to validate your installation is correct.

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

## Installing MongoDB

1. Download and install from [MongoDB](https://www.mongodb.com/download-center#community)
2. Press Window + S, type 'environment' and select 'edit the system environment variables'
3. Click 'environment variables' near the bottom the of pop up window
4. Click 'Path' at the top of window
5. Depending on how many paths you already have set up, you either:
  1. click New and write "C:\Program Files\MongoDB\Server\3.4\bin"
  2. have a list that is seperated by semicolons; so add another semicolon to the end of the list and add the path as abovementioned.
6. You should now be able to run `mongodb` from git bash CLI
