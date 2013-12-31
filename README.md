# Setting up Ruby, Rails, Nginx, Passenger, PostgreSQL 9 on Ubuntu 12.04 LTS for Windows Azure
**Last updated: 12/31/2013**

## Fix the locale issue
* Edit `/etc/default/locale` as sudo.
* Append `LC_ALL="en_US.UTF-8"` at the end of the file, save and quit.
* `sudo locale-gen en_US en_US.UTF-8`
* `sudo dpkg-reconfigure locales`

## Install the necessary packages to install rbenv and build Ruby
* `sudo apt-get update`
* `sudo apt-get -y install build-essential bison openssl libreadline6 libreadline6-dev curl git-core`
* `sudo apt-get -y install zlib1g zlib1g-dev libssl-dev libyaml-dev libsqlite3-0 libsqlite3-dev sqlite3`
* `sudo apt-get -y install libxml2-dev libxslt-dev autoconf libc6-dev ncurses-dev libcurl4-openssl-dev`

## Install rbenv
* `curl https://raw.github.com/fesplugas/rbenv-installer/master/bin/rbenv-installer | bash`
* Append this at the end of `~/.bashrc`:

```
export RBENV_ROOT="${HOME}/.rbenv"

if [ -d "${RBENV_ROOT}" ]; then
  export PATH="${RBENV_ROOT}/bin:${PATH}"
  eval "$(rbenv init -)"
fi
```

* `git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build`
* `git clone git://github.com/dcarley/rbenv-sudo.git ~/.rbenv/plugins/rbenv-sudo`
* `source ~/.bashrc`

## Install Ruby
* `rbenv install 2.1.0`
* `rbenv rehash`
* `rbenv global 2.1.0`

## Install Rails
* `gem install bundler --no-ri --no-rdoc`
* `gem install rails -v 4.1.0.beta1 --no-ri --no-rdoc`
* `rbenv rehash`

## Install Node.js
* `sudo add-apt-repository ppa:chris-lea/node.js`
* `sudo apt-get update`
* `sudo apt-get -y install nodejs`

## Install Passenger and nginx
* `gem install passenger --no-ri --no-rdoc`
* `rbenv rehash`
* `rbenv sudo passenger-install-nginx-module`
* `wget -O init-deb.sh http://library.linode.com/assets/660-init-deb.sh`
* `sudo mv init-deb.sh /etc/init.d/nginx`
* `sudo chmod +x /etc/init.d/nginx`
* `sudo /usr/sbin/update-rc.d -f nginx defaults`
* `sudo service nginx start`

## Install PostgreSQL
* `sudo add-apt-repository ppa:pitti/postgresql`
* `sudo apt-get update`
* `sudo apt-get -y install postgresql libpq-dev`
* `sudo passwd postgres` and enter password 2 times
* `su -l postgres`
* `psql`

```
psql (9.1.11)
Type "help" for help.

postgres=# ALTER USER postgres WITH PASSWORD 'secr3t';
ALTER ROLE
postgres=# CREATE ROLE postgres LOGIN CREATEDB
postgres-# \q
```

* `/etc/init.d/postgresql restart`