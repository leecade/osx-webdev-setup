# The Perfect Web Development Setup for OS X

This is what I perceive as the perfect web development OS X setup.
It makes web development a breeze.
I've tried to use Ubuntu, but unfortunately it's just not as user friendly as OS X.
I wish to make one for Ubuntu eventually once I figure it out.

Note: this is all from memory and will be revised once I get a new Mac or reformat this one.
Clarifications and PRs are welcomed.

## Goals

- Never use `sudo`. Once you run a command with `sudo`, future commands are probably gonna be fucked up as well.
- Automatically start all services. Don't bother keeping track a bunch of terminals running processes.
- Don't shoot yourself in the foot.

## Environment Profile

My current profile is `~/.bash_profile`.
I don't know why OS X uses `~/.bash_profile`,
but it may be different for you.
Some lines you should add are:

- `ulimit -n 10240` - bumps the maximum number of file descriptors you can have open on your computer.
  There's no purpose for the default limit, especially on SSDs.
- `shopt -s globstar` - support `**/*.js` commands.
- `export JOBS=max` - tells `npm` to compile and install all your native addons in parallel and not sequentially.
  This greatly increases installation times.

## XCode Command Line Tools

First, you need to install XCode's command line tools.
This installs a lot of tools like `git` which aren't needed for plebeians. 

```bash
xcode-select --install
```

This command may actually require you to use `sudo`.

## Install Homebrew

[Homebrew](http://brew.sh/) is OS X's package manager.
It makes setting up all your services very easy.

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## Install everything

Install everything with Homebrew.
Here are some packages you might be interested in right now:

```bash
# for editing files
brew install vim
# always keep your git up to date by installing it with brew
brew install git
# download stuff
brew install curl
# for your pr0n
brew install youtube-dl
```

### Updating

To update your packages,
simply run:

```bash
brew update
brew upgrade
```

Once in a while, I run `brew prune` and `brew doctor` to keep my computer in check.

## Install node.js with nvm

One thing many users do is install node.js globally.
This is easy to get started or fine for servers,
but it makes developing a pain.
If you have to ever run `npm` with `sudo`, 
you're doing it wrong!

[nvm](https://github.com/creationix/nvm) is what I think is the best node version manager.
It can be installed with homebrew!
Yes, you use a package manager to install a version manager to install another package manager.
It's stupid, but they all have their strengths.

```bash
brew install nvm
```

Once you've installed nvm,
install the version of node.js you use:

```bash
nvm install 2
```

To make sure each terminal uses the version of node you want,
add this to your `~/.bash_profile` or whichever environment you use:

```env
# load nvm whenever a terminal starts
source ~/.nvm/nvm.sh
# load the version of nvm you want
nvm use 2
```

Now every time you open a window,
it will say which version of node you are using.
This might be annoying, 
but it's better than not knowing what version you're using!

### Using npm

Don't ever use `sudo`!
You don't need to `sudo npm install -g grunt-cli` or anything anymore.
Just run `npm install -g eslint` and `eslint` will always be in your path.
`nvm` adds these to your `$PATH`.

Don't ever upgrade `npm`!
At least not until node.js and io.js merge.
If anyone ever tells you to `npm update -g npm` or `npm install -g npm`,
tell them to shuttup.

## thefuck?

[thefuck](https://github.com/nvbn/thefuck) is a nifty tool that allows you to fix your previous CLI typos by just typing `fuck`.

Installing it is easy:

```bash
brew install thefuck
```

Then alias it as `fuck` (or whatever you want) manually by adding this line to your `~/.bash_profile`:

```env
alias fuck='$(thefuck $(fc -ln -1))'
```

## git

`git` by default doesn't have autocompletion on OS X.
Super easy to [install it](https://github.com/bobthecow/git-flow-completion/wiki/Install-Bash-git-completion):

```bash
brew install bash-completion
```

Then add this line to your `~/.bash_profile`:

```bash
if [ -f `brew --prefix`/etc/bash_completion ]; then
    . `brew --prefix`/etc/bash_completion
fi
```
