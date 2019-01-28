# Windows Installfest 3

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
	- `gem update`
	- `gem update --system`
	- `gem install rails` (this can take a long time, ~10 minutes)
	- `rbenv rehash`
	- `rails -v`


