# Installation Notes for Windows 10

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

## Node

We will install the Linux version of NodeJS into WSL without any version manager. The commands here will install Node v10.x. For other versions, change the number after `setup_` in the 2nd command accordingly. If you wish to run multiple versions of NodeJS on your machine, which may be necessary for legacy development work, look at the section on using nvm instead. __Warning: Do NOT run through both sets of instructions (here, and the one for `nvm`) or you will encounter version/library conflicts.__

- Run the following commands in order in the WSL terminal. Wait for each to complete before running the next.
	- `sudo apt-get update && sudo apt-get upgrade`
	- `curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -`
	- `sudo apt-get install -y nodejs`
  - `mkdir ~/.npm`
  - `npm config set prefix '~/.npm'`
  - `echo 'export PATH="$HOME/.npm/bin:$PATH"' >> ~/.bashrc`
  - `source ~/.bashrc`
	- `node -v` (Checks the installed version of NodeJS)
	- `npm -v` (Checks the installed version of npm)

## Installing NodeJS using the Node Version Manager (nvm)
This method allows you to use different versions of NodeJS if required, but incurs a WSL performance penalty as the NVM takes some time to start up each time you open a WSL window.
- Run the following commands in order at the WSL terminal.
	- `curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash`
	- `nvm install node` (may take very long if your computer has to compile node)


#### GitHub
Run the following commands in order in a WSL terminal.
- `sudo apt install git`
- `git config --global credential.helper cache` This will cache your git credentials for a short time after you enter it.
- If you wish to extend the amount of time for which your git credentials are cached, run `nano ~/.gitconfig` and edit the line `helper = cache` to `helper = cache --timeout=86400`. The number after `--timeout=` refers to the cache duration in seconds, so the previous line would cache your credentials for 1 day.

## Installing PostgreSQL
- Any database system contains at least 2 parts: the database server itself, and the database client. As WSL does not officially support GUI tools, for our convenience, we will install database servers on Windows, and use command line clients in WSL. This allows us to optionally install GUI clients in Windows for more convenient data visualization.

- __Installing the PostgreSQL server__
	- Download and install the latest version for Windows x86-64 from (https://www.enterprisedb.com/downloads/postgres-postgresql-downloads). This will also install PGAdmin.
	- Use pgAdmin 4 to create a `Login/Group Role` for your server with the __same name as your WSL username__. Ensure that under the `Privileges` tab, the options for `login` and `create databases` are turned on.
	- Use pgAdmin 4 to create a `Database` with the __same name as your WSL username__, and set its owner to the user that you just created.

- __Installing the PostgreSQL client__
	- Open a WSL terminal and run the following commands in order.
	- `sudo apt-get update && sudo apt-get upgrade`
	- `sudo apt-get install postgresql-client-10 postgresql-client-common` (Change the version after -client- to the version of PostgreSQL that you installed)
	- `echo 'export PGHOST=localhost' >> ~/.bashrc`
	- Either `source ~/.bashrc` or restart WSL to reload the config
	- `psql` to check that everything runs. Type `\l` to see a list of databases. Hit `q` to exit the list if needed, and type `\q` to exit the client.
	- If you want PostgreSQL to start automatically on boot, do the following.
		- Press the Win key and type `services`. Press enter.
		- Look for the PostgreSQL 10 Server service, right-click on it, and ensure that the `Startup type:` is set to `Automatic`.

- __HeidiSQL__
  - Go to the Microsoft Store, search for and install `HeidiSQL`. This is a much more user friendly graphical SQL client. Originally meant for MySQL installations (another type of SQL server), it now works for PostgreSQL as well.

## Installing Ruby
- Run the following commands in order in the WSL terminal.
	- `sudo apt-get uninstall ruby` This ensures that you will not be using any default version of Ruby that might have already been pre-installed and is likely old.
	- `sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev software-properties-common libffi-dev`
	- `git clone https://github.com/rbenv/rbenv.git ~/.rbenv`
	- `echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc`
	- `echo 'eval "$(rbenv init -)"' >> ~/.bashrc`
	- `exec $SHELL`
	- `git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build`
	- `echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc`
	- `exec $SHELL`
	- `rbenv install 2.5.1` (this can take a long time, ~10-30 minutes)
	- `rbenv global 2.5.1`
	- `ruby -v`

## Installing Rails
- Install Ruby first as above, then run the following commands.
	- `gem install rails` (this can take a long time, ~10 minutes)
	- `rbenv rehash`
	- `rails -v`

## Installing MongoDB
- Get and install MongoDB Community Edition from (https://www.mongodb.com/download-center#community). Optionally, grab the MongoDB Compass, a GUI frontend for MongoDB, from (https://www.mongodb.com/download-center#compass) and install it __after__ all the steps here are done.
- We need to create the directories for MongoDB to store data and logs in.
	- Press the Windows key and type `cmd.exe` and press `Ctrl-Shift-Enter` to run it as an administrator. Run the following commands.
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `mkdir c:\data\db`
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `mkdir c:\data\log`
	- Use Explorer to navigate to `C:\Program Files\MongoDB\Server\3.6\` and create a file called `mongod.cfg`. Open it with notepad and type the following:
		`systemLog:`
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `destination: file`
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `path: c:\data\log\mongod.log`
		`storage:`
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `dbPath: c:\data\db`
	- Return to the Command Prompt and run the following.
		- `"C:\Program Files\MongoDB\Server\3.6\bin\mongod.exe" --config "C:\Program Files\MongoDB\Server\3.6\mongod.cfg" --install`
		- `net start MongoDB`
	- Open WSL and run the following commands
		- `sudo apt-get update && sudo apt-get upgrade`
		- `sudo apt-get install mongodb-clients mongodb-tools`
		- `mongo` to check that everything runs. Type `quit()` to exit the client.
	- If you want MongoDB to start automatically on boot, i.e. you won't have to type `net start MongoDB` each time you want to start on your dev session, do the following.
		- Press the Win key and type `services`. Press enter.
		- Look for the MongoDB service, right-click on it, and ensure that the `Startup type:` is set to `Automatic`.