# Github book editor

The book editor is a javascript browser application that allows editing an
ebook in your browser and storing the ebook itself in a github repo.

## Technologies used 
* [Backbone.js](http://backbonejs.org/)
* [Marionette](http://marionettejs.com/)
* [Bootstrap](http://getbootstrap.com/)
* [Font Awesome](http://fontawesome.io/)
* [Jquery](http://www.jquery.com/)
* [Requirejs](http://requirejs.org/) and helpers such as
  [require-css](https://github.com/guybedford/require-css) and
  [require-less](https://github.com/guybedford/require-less)
* Phil Schatz's [Octokit.js](https://github.com/philschatz/octokit.js/)
* Our own [OERPub specific version of Aloha-Editor](https://github.com/oerpub/Aloha-Editor/)
* [NodeJS](http://nodejs.org/) and associated development tools such as npm
  and [Bower](https://github.com/bower/bower).

## Installation with Vagrant
* install [virtualbox](https://www.virtualbox.org/wiki/Downloads)
* install [vagrant](http://downloads.vagrantup.com/)
* clone [github book editor](https://github.com/oerpub/github-bookeditor) repo
  to somewhere
* inside the repo run `vagrant up` from a command line
  * There is currently a bug in the build that makes it not run fully on the first pass, the workaround is to log into the vm after running `vagrant up` with `vagrant ssh`, then go into `/vagrant` and run `npm install`. That will complete the build for you.
* Vagrant will take a while to configure the new vm when it's done you will be able to hit "33.33.33.10" in a web browser and see the editor

## Installation on Ubuntu Linux

### Installing node.js

First you need to install a copy of nodejs and the required utilities. Because
the version of nodejs that ships with your favourite linux distribution is
likely out of date, the easiest way is to install a basic development
environment and compile nodejs from source:

    sudo apt-get update
    sudo apt-get install build-essential openssl libssl-dev pkg-config

Now download the source tarball and compile it:

    cd /tmp
    wget http://nodejs.org/dist/v0.10.22/node-v0.10.22.tar.gz
    tar zxf node-v0.10.22.tar.gz
    cd node-v0.10.22
    ./configure --prefix=$HOME/nodejs
    make && make install

### Installing coffeescript

Once you have node installed, you can use npm to install the rest of what you
need. You will need coffee script and lessc if you're going to hack on the
Aloha code.

    ~/nodejs/bin/npm install -g coffee-script
    ~/nodejs/bin/npm install -g lessc

### Installing the book editor

Next clone the bookeditor repo. For this example we will clone it into your
home directory:

    cd $HOME
    git clone git@github.com:oerpub/github-bookeditor.git

Then use npm to install all dependencies:

    ~/nodejs/bin/npm install

### Setting up a web server

The simplest way to set this up is to install apache:

    sudo apt-get install apache2

Then symlink your development directory into /var/www:

    sudo ln -s $HOME/github-bookeditor /var/www/github-bookeditor

You can now access the application in your browser at `http://localhost/github-bookeditor/`.

## Code layout

Except for the technologies listed above, the book editor itself is made up
of three other products that each live in its own repository. The editor itself
started its live as [atc](https://github.com/Connexions/atc/), but was soon
used as the basis for the a github based [book editor](https://github.com/oerpub/github-bookeditor/).

At the time of writing, [work is in progress
](https://github.com/oerpub/github-bookeditor/pull/115) to make atc
generic enough that github-book would consist of only a hand full of extensions
and replacements.

A further progression in this process would be to [split the common part from
atc](https://github.com/oerpub/github-bookeditor/pull/115#issuecomment-28458218)
so that atc and the bookeditor can each have their own customisations.

You should find all the relevant components in the `bower_components` directory
inside the github-bookeditor checkout.

## Developing on components in bower_components.

The checkouts in `bower_components` are unfortunately not real git clones, so
the simplest way to develop on these is to make a checkout somewhere else and
symlink it. For example, if I want to develop on Aloha-Editor, I would link it
like this:

    cd $HOME
    git clone git@github.com:oerpub/Aloha-Editor.git
    cd github-bookeditor/bower_components
    rm -rf aloha-editor && ln -s $HOME/Aloha-Editor aloha-editor

This will create a symlink in bower_components that point to a real checkout
in $HOME/Aloha-Editor, allowing you to do development in a familiar setting.

## Running tests

We don't presently have a testing framework. We really should have one.
