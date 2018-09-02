# Windows Installfest 2

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
	- `sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libpq-dev libxml2-dev libxslt1-dev libcurl4-openssl-dev software-properties-common libffi-dev`
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
