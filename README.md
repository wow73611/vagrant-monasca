Install's a mini monitoring environment based on vagrant. Intended for development and monitoring of the monitoring infrastructure.

# Usage

## Get the Code

```
git clone https://git.hpcloud.net/mon/mini-mon.git
```

## Setup Vagrant

### Install Vagrant
Assumes you have home homebrew installed, if not download and install VirtualBox and Vagrant from their websites then continue  with Setup Berkshelf.

```
brew tap phinze/cask
brew install brew-cask
brew cask install virtualbox 
brew cask install vagrant
```

### Setup Berkshelf
```
vagrant plugin install vagrant-berkshelf
gem install berkshelf  or gem install --http-proxy <http://some-proxy.foo.com:8088> berkshelf
```

# Using mini-mon

- Your home dir is synced to `/vagrant_home` on each vm
- Vms created
  - `api` at `192.168.10.4`
  - `kafka` at `192.168.10.10` - mon-notification runs on this box also
  - `mysql` at `192.168.10.6`
  - `persister` at `192.168.10.12`
  - `thresh` at `192.168.10.14`
  - `vertica` at `192.168.10.8`
    - The management console is at https://192.168.10.8:5450
- Run `vagrant help` for more info
- Run `vagrant ssh <vm name>` to login to a particular vm
- Can also run `ssh vagrant@<ip address>` to login 
  - password is `vagrant`
  
## Start mini-mon
Berkshelf will download some cookbooks from the community so http_proxy and https_proxy environment variables must be set if applicable.
From within the `mini-mon` directory, to start all the vms run:
```
bin/vup
```
The standard vagrant commands can also be used, the vup script just starts things up in a dependency order using parallel startup.

## Halt mini-mon
In some cases halting mini-mon can result in certain vms being left in an odd state, to avoid this a script has been made to halt boxes in the 
correct order
```
bin/vhalt
```

## Updating a VM
When someone updates the config for a vm this process should allow you to bring up an updated vm.
- `git pull`
- `berks update`
- `vagrant destroy vm` - Where vm is the name of the vm being updated, for example 'vertica'
- `vagrant up vm`

## Improving Provisioning Speed
The slowest part of the provisioning process is the downloading of deb packages. To speed this up a local apt-cacher-ng can be used.
To install on a mac
```
brew install apt-cacher-ng
```
Run `apt-cacher-ng -c /usr/local/etc/apt-cacher-ng/` or optionally follow the instructions from brew to start up the cache automatically.
That is all that is needed from now on the cache will be used.

A report from the cache is found at http://localhost:3142/acng-report.html

# Creating a new hlinux box
The [hlinux](http://hlinux-home.usa.hp.com/wiki/index.php/Main_Page) box used in mini-mon is created via a [veewee](https://github.com/jedi4ever/veewee)
template in the templates directory.

- Follow the instructions at the [veewee](https://github.com/jedi4ever/veewee) site to install veewee.
  - Copy `templates/Veeweefile` to your VeeWee directory and edit the file, updating the path to your mini-mon.
  - At this point the template assumes you need an hp proxy setup, if not edit base.sh and comment out the .curlrc lines at the bottom
  - From your veewee directory run:
    - `bundle exec veewee vbox define hlinux hlinux-amd64-netboot`
    - `bundle exec veewee vbox build hlinux`
    - `bundle exec veewee vbox validate hlinux`
    - `bundle exec veewee vbox export hlinux`
- From the mini-mon directory run `vagrant box add hlinux ../veewee/hlinux.box` or other appropriate path, also upload to a server for others to download.
  - If you have an existing hlinux box you man need to first remove it `vagrant box remove hlinux`
