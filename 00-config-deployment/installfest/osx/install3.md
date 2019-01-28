# Installfest 3

## Installing Ruby on Rails

### Install rbenv
rbenv lets us change ruby verions on the fly, useful for working with diffrent versions.

```
brew update
brew install rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.profile
echo 'eval "$(rbenv init -)"' >> ~/.profile
source ~/.profile

sudo chown -R $USER ~/.rbenv
```

### Configuring rbenv
```
brew update

brew install ruby-build

rbenv install 2.5.1
rbenv global 2.5.1
```

### Install Rails

```
gem update
gem update --system
gem install rails
```
You may need to press "yes" for various entries

### Verify your installation

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
```

## Install some other sublime packages:
  * Sass (syntax definition, we'll use this when working with Rails)
