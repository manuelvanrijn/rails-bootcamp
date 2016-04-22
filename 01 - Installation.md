# Installation

### Table of content

1. [Setup your machine](#1-setup-your-machine-osx)
  * [Install (Home)brew, rbenv and ruby-build](#11-install-homebrew-rbenv-and-ruby-build)
  * [Test if everything works](#test-if-everything-works)
  * [Bonus](#bonus)
2. [Installing a new Ruby](#12-installing-a-new-ruby)
  * [Upgrade ruby-build to contain the latest Rubies](#121-upgrade-ruby-build-to-contain-the-latest-rubies)
  * [Finding a Ruby version](#122-finding-a-ruby-version)
3. [Set a Ruby version as your new global version](#3-set-a-ruby-version-as-your-new-global-version)
  * [The magic `.ruby-version` file](#31-the-magic-ruby-version-file)
  * [Bonus](#bonus_1)
4. [Recap](#recap)

Be default osx is shipped ruby (El Capitain is shipped with version `2.0.0p648`), but as you work on multiple Rails applications, it's certain that you have to work with different versions of Ruby.

Just like Java, Swift e.d. newer versions, contain newer functions and/or deprecated once.

To be able to manage multiple Ruby versions on your machine, we use a tool called [rbenv](https://github.com/rbenv/rbenv) in combination with the [ruby-build](https://github.com/rbenv/ruby-build) plugin, to be able to install and manage multiple versions of Rubies

## 1. Setup your machine (osx)

The easiest way to install [rbenv](https://github.com/rbenv/rbenv) and [ruby-build](https://github.com/rbenv/ruby-build) is to install [Homebrew](http://brew.sh/) which like `apt-get` for Ubuntu only than for osx

### 1.1 Install (Home)brew, rbenv and ruby-build

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install rbenv ruby-build
rbenv init
```

This last command, will show you what changes you have to make to initialize `rbenv` for the shell you use.

#### Test if everything works

Try running `rbenv versions` which returns a list of installed Ruby versions. You'll see `system` which is the default Ruby version of OSX

#### Bonus

Before we install a new Ruby version with `rbenv` I would recommend to install a plugin called [rbenv-default-gems](https://github.com/rbenv/rbenv-default-gems).
This plugin adds the function to install some default gems, when you install a new version of Ruby (we'll cover gems later on)

So for all Rails projects, we use a gem called `bundler` so this is typically a gem we always want to install after we've installed a new Ruby version.

Run the following commands:

```
brew install rbenv-default-gems
echo 'bundler' >> $(rbenv root)/default-gems
```

## 1.2 Installing a new Ruby

### 1.2.1 Upgrade ruby-build to contain the latest Rubies

The following step is only needed, when ruby-build is outdated and isn't containing the version of Ruby you want to install.

```
brew update && brew upgrade ruby-build
```

### 1.2.2 Finding a Ruby version

We are going to install the latest Ruby version on our system. To find out what versions are available run:

```
rbenv install --list
```

This results in something like:

```
1.8.6-p383
1.8.6-p420
1.8.7-p249
1.8.7-p302
... more ...
2.2.4
2.3.0-dev
2.3.0-preview1
2.3.0-preview2
2.3.0                     <--- Latest none dev release of ruby
2.4.0-dev
jruby-1.5.6
... more ...
jruby-9.0.4.0
jruby-9.0.5.0
... more ...
maglev-1.0.0
rbx-3.13
rbx-3.14
ree-1.8.7-2011.03
ree-1.8.7-2011.12
ree-1.8.7-2012.01
ree-1.8.7-2012.02
topaz-dev
```

> I'm not going to explain the differences between the multiple interpreters ruby-build offers us, because we will not use these for our day to day Ruby on Rails development, but if you want to read more about these check out the [rvm interpreters page](https://rvm.io/interpreters)

As we see in the list the latest (none dev build) is version `2.3.0`. To install this version run:

```
rbenv install 2.3.0
```

As you'll notice, this will take some time to complete, because the version of ruby is being build, compiled and installed on your system. After this process is completed, run `rbenv versions` to see if the `2.3.0` build is added.

## 3. Set a Ruby version as your new global version

At this point when we run `ruby -v` we'll still get the version number of our `system` ruby. This is because `rbenv` has marked the `system` ruby as the `global` ruby version to use if there isn't a file called `.ruby-version` present (be patient, we'll cover this file later on)

To make our installed ruby 2.3.0 version our new `global` version run:

```
rbenv global 2.3.0
```

And test it out by running `ruby -v` once again.

### 3.1 The magic `.ruby-version` file

Alright, so there's some magic within `rbenv` that looks for this file in the current and parents folder to determine which version of ruby it should load.

It's a good practice to add a `.ruby-version` file, containing the version number of the Ruby you used to develop the project in.

For example:
My system contains additional Ruby version, `2.1.0`
When I go to a folder containing a `.ruby-version` file containing `2.3.0`, `rbenv` tries to switch to this version of Ruby. If this version isn't installed you'll get the following message:

```
rbenv: version `2.3.0' is not installed (set by ~/Projects/the-project/.ruby-version)
```

#### Bonus

Instead of creating a `.ruby-version` file manually, to commit to `git` you can run:
```
rbenv local 2.3.0
```

which will create the `.ruby-version` file in the folder you're in, containing `2.3.0`

## Recap

So what have we learned?

1. How to install `rbenv` with the plugins `ruby-build` and `rbenv-default-gems` using `brew`
2. How to install a new version of Ruby
3. How to set a installed version of Ruby as your new global version
4. How `rbenv` handles switching between different ruby versions using the `.ruby-version` file
