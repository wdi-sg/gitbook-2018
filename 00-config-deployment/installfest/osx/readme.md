# WDI Singapore Install Fest

For the first portion of the class, we'll be working exclusively inside of the browser. We'll be installing the following tools.

* Slack
* Homebrew
* Git

If you already have some tools and software installed that are similar to below, it will be more conveient for you to switch over than it will be for you to try to go ahead with your current versions.

## Hidden Files
With a finder window open, set your finder to display hidden unix files by default:
```
cmd + shift + .
```

## Slack

We will be using slack to communicate throughout the course. You should've received an invite to our channels via e-mail. You can login via the web browser, but downloading / installing the app is highly recommended.

[Download Slack](https://slack.com/downloads)

## Homebrew

Homebrew is a package manager that we will use to install various command line tools in our class.

Open up terminal, and paste the following command to install Homebrew. You might be prompted to install XCode Command Line Tools during the install process.

```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

You may be prompted to installed XCode command line tools. When prompted, click and install through that, and you're homebrew installation will continue.

After the installation process, run the command `brew doctor`. If any warnings or errors are displayed, we will need to resolve them before proceeding with the rest of the install fest.

## Xcode

Speaking of Xcode, install Xcode through the App Store. [Link here](https://itunes.apple.com/us/app/xcode/id497799835?mt=12)

## GIT
Before we do this process, please make sure you have signed up for an account on [Github](http://www.github.com). We will be installing a version of GIT from home brew and also configuring it.

To install GIT
```
brew install git
```

#### Configuring GIT

Using your email credentials for GIT, run these commands with your user and email configured.

```
git config --global user.name "YOUR-USERNAME"
git config --global user.email "YOUR-EMAIL-ADDRESS"
git config --global push.default simple
git config --global credential.helper osxkeychain
```

You should probably install the command line prompt and autocompletition plugins:
[https://git-scm.com/book/en/v2/Appendix-A%3A-Git-in-Other-Environments-Git-in-Bash](https://git-scm.com/book/en/v2/Appendix-A%3A-Git-in-Other-Environments-Git-in-Bash)

#### Caching Github Login
We'll mainly be using HTTPS, so use a credential helper to cache our keys. You should already be setup if you used homebrew to install git, else follow these steps: https://help.github.com/articles/caching-your-github-password-in-git/#platform-mac

### Setting up the bash shell
> do you have any other shell configuration files in your home directory?
> `ls -la ~`
> If you have something named `.zshrc`, `.bashrc`, `.bash_profile`
> Take the contents out of this file and put it in the new one we are creating. Then delete the old file.

Create a new shell config file.
```
touch ~/.profile
```


## Sublime
We'll be running **Sublime**, as our text editor of choice.

Download and install the latest version [https://www.sublimetext.com/](https://www.sublimetext.com/)

#### Run sublime from your command line

Create a bash alias and put it in your profile config file:
```
echo 'alias sublime="/Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl"' >> ~/.profile
```

**Testing**
Open a Terminal window and run:
```
sublime ~/Documents
```
**to open the entire current directory**
```
sublime .
```

#### set sublime as your command line default editor:

Open your ~/.profile file using sublime.

Add the code below:

**this also allows you to refresh changes to your shell config**
```
export VISUAL=sublime
export EDITOR="$VISUAL"

function subledit() {
  sublime ~/.profile
}

function profilerefresh() {
  echo "Refreshing your configuration."
  source ~/.profile
}
```

### Install the sublime `packagecontrol` library
Package Control allows you to add new functionality to sublime.

[https://packagecontrol.io/installation](https://packagecontrol.io/installation)


### Get the `Preferences.sublime-settings` file:
[https://raw.githubusercontent.com/wdi-sg/gitbook-2018/master/Preferences.sublime-settings](https://raw.githubusercontent.com/wdi-sg/gitbook-2018/master/Preferences.sublime-settings#)

Create a file named `Preferences.sublime-settings` exactly, and save it in your home directory.

#### Using Package Control
[https://packagecontrol.io/docs/usage](https://packagecontrol.io/docs/usage)

> Package Control is driven by the Command Palette. To open the palette, press `ctrl+shift+p` (Win, Linux) or `cmd+shift+p` (OS X). All Package Control commands begin with Package Control:, so start by typing Package.

### Set Sublime to use `editorconfig` files
- `cmd+shift+p` type in `Package Control: Install Package` (auto-complete will help you) and press return
- type in `editorconfig` to install the package

### Get the `.editorconfig` file:
[https://raw.githubusercontent.com/wdi-sg/gitbook-2018/master/.editorconfig](https://raw.githubusercontent.com/wdi-sg/gitbook-2018/master/.editorconfig)

Create a file named `.editorconfig` exactly, and save it in your home directory.

### Get the `.gitignore file`
[https://raw.githubusercontent.com/wdi-sg/gitbook-2018/master/.gitignore](https://raw.githubusercontent.com/wdi-sg/gitbook-2018/master/.gitignore)

Create a file named `.gitignore` exactly, and save it in your home directory.


### Set Sublime to run as your git commit message editor

#### BEFORE YOU RUN THIS, MAKE SURE THE PATH / LOCATION OF YOUR SUBLIME APP IS CORRECT
```
git config --global core.editor "/Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl -w"
```

## Install some other sublime packages:
  * ColorPicker (pick colors by typing `COMMAND + SHIFT + c`, handy for CSS)
  * Color Highlighter (visually displays colors for hex/rgb values)
  * GitGutter (shows git additions/deletions)
  * BracketHighlighter (highlight brackets and tabs)
