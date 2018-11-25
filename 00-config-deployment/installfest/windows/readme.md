# Windows Installfest

For the first portion of the class, we'll be working exclusively inside of the browser. We'll be installing the following tools.

* Slack
* Git
* WSL

If you already have some tools and software installed that are similar to below, it will be more conveient for you to switch over than it will be for you to try to go ahead with your current versions.

## Sublime
We'll be running **Sublime**, as our text editor of choice.

Download and install the latest version [https://www.sublimetext.com/](https://www.sublimetext.com/)

### Install the sublime `packagecontrol` library
Package Control allows you to add new functionality to sublime.

[https://packagecontrol.io/installation](https://packagecontrol.io/installation)


### Get the `Preferences.sublime-settings` file:
[https://raw.githubusercontent.com/wdi-sg/gitbook-2018/master/Preferences.sublime-settings](https://raw.githubusercontent.com/wdi-sg/gitbook-2018/master/Preferences.sublime-settings#)

Open sublime and use the menu bar to go to: Sublime Text -> Preferences -> Settings

On the right window (Preferences.sublime-settings), earse the current contents of the file and paste in the entire contents of the file from the link above.


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


## Installation Notes for Windows 10

A 64-bit version of Windows 10 is absolutely needed here as we will be using the Windows Subsystem for Linux (WSL) extensively. You can check whether your version of Windows 10 is a 64-bit one by going to `Settings -> System -> About` and looking under the `System type` field.

## Installing the Windows Subsystem for Linux (WSL)
[https://docs.microsoft.com/en-us/windows/wsl/install-win10](https://docs.microsoft.com/en-us/windows/wsl/install-win10)

- Press the Windows key and search for `Windows features` and select `Turn Windows features on and off`.
- Scroll down and find `Windows Subsystem for Linux` and make sure it's checked, then press OK. Restart the system when prompted to.
- Next, go to the Windows store and search for `Ubuntu` and install it.
- Launch Ubuntu from the start menu. Enter your Linux username and password, and make a note of it (they are needed later). Under no circumstances should you leave these blank or cancel the process, as this will run your Linux installation as a root user which causes permission warnings later on and also poses a significant security risk to your system.
  - _Do not forget your Linux password. It is needed for admin operations in the WSL environment._
- _Note: Right-clicking on the WSL window pastes whatever is in your clipboard. This can save you some time when running the longer commands here._

## Symlinking a Windows workspace folder into your WSL home
- __Warning__: Your WSL files are stored in a separate file system managed by WSL with a different, stricter and more fine-grained permission system than Windows. You should _never_ edit any WSL files from Windows itself, as there is a non-trivial risk of corrupting your entire WSL installation. However, editing Windows files from WSL is perfectly fine. Thus, we can integrate the two systems (WSL and Windows) by making sure that your working folders live in Windows and are conveniently accessible from WSL.

- Create a `projects` folder from your Windows user account home directory. For example, go to `C:\Users` in Explorer and go into the folder corresponding to your user name.  Create a folder named `projects` here. For example, if your home directory is `C:\Users\Bobby Tan`, create the directory `C:\Users\Bobby Tan\projects`. All projects should be created in this folder so that you can safely browse or edit the files from both WSL and Windows.
- Next, symlink it by opening a WSL window and running the following commands in order
	- `cd ~`
	- `ln -s /mnt/c/Users/Bobby\ Tan/projects ./projects`
	
- From now on, you will be able to access the projects folder in your WSL installation as if it were a directory in it.

- Install your text editor of choice in Windows, e.g. Sublime Text (https://www.sublimetext.com/3) or VSCode (https://code.visualstudio.com/download). 

- Create an alias for your text editor in WSL so that you can launch it from the WSL's CLI. For Sublime Text, if you had installed it at the default location, run `echo 'alias subl="/mnt/c/Program\ Files/Sublime\ Text\ 3/subl.exe"' >> ~/.profile` at your WSL's CLI. Then, close and re-open WSL, or run `source ~/.bashrc` to reload the configuration, and test it out by typeing `subl` and pressing enter in WSL.

## (Optional) Speeding up WSL's I/O Performance

WSL's disk I/O speeds are quite abysmal out-of-the-box as of this writing due to W10's real-time antivirus protection. However, this protection is fairly useless for Linux, so we can gain quite a bit of speed from disabling it for (only) WSL. Follow the instructions over at https://medium.com/@leandrw/speeding-up-wsl-i-o-up-than-5x-fast-saving-a-lot-of-battery-life-cpu-usage-c3537dd03c74 to do so. The performance improvement is most noticeable when developing with Rails.

#### GitHub
Run the following commands in order in a WSL terminal.
- `sudo apt install git`
- `git config --global credential.helper cache` This will cache your git credentials for a short time after you enter it.
- If you wish to extend the amount of time for which your git credentials are cached, run `nano ~/.gitconfig` and edit the line `helper = cache` to `helper = cache --timeout=86400`. The number after `--timeout=` refers to the cache duration in seconds, so the previous line would cache your credentials for 1 day.

