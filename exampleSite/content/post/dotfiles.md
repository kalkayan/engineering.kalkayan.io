+++ 
author = "Manish Sahani" 
title = "Hades's Dotfiles" 
date = "2019-03-11" 
description = "" 
tags = [ "markdown", "css", "html", ] 
categories = [ "themes", "syntax", ] 
series = ["Themes Guide"] 
aliases = ["migrate-from-jekyl"] 
+++

Many nights of potential productive work programmers have lost by procrastinating on configuring their dotfiles. These simple invisible files may seem pointless for a novice, but they become a swiss army knife if properly configured. I recently came across an elegant approach to manage these dotfiles with no extra tooling, no symlinks, just a bare git repo. 


![banner.jpe](https://raw.githubusercontent.com/kalkayan/dotfiles/main/static/banner.jpg)

### Hades's Dotfiles 

There is something fascinating about customizing your operating system through dotfiles, and there are tons of articles on what you can do with these dotfiles. This repository is the collection of configurations that I learned over time and still use for my daily work. I primarily use a 13' Macbook pro (named hades) for coding; therefore, this repository mainly applies to macOS, but Improvements or contributions are more than welcome. 

Feel free to reach me out at [rec.manish.sahani@gmail.com](mailto:rec.manish.sahani@gmail.com) or connect with me on [LinkedIn](https://www.linkedin.com/in/manishsahani).


### To start using these dotfiles

If you want to give this a try, first fork the repository, **review the files and code**, and **remove code that you don't need**.
<!-- ([see - Reviewing & editing code ]()). -->

> :warning: Don't blindly use these settings unless you know what that entails.

This repository container a bash script (`setup`) to automate the installation of all the binaries and brew casks; not only this but the repository also act as a dotfiles manager if followed the instructions below.

#### Managing & tracking [dot]files

The trick to managing these dotfiles is by creating a [bare](https://www.atlassian.com/git/tutorials/setting-up-a-repository/git-init) git repository. To use this repository, clone this with `--bare` option and source the `.zshrc`, run the following command in the terminal:

```bash
# after forking replace the <username> with your github handle
git clone --bare https://github.com/<username>/dotfiles.git $HOME/.dotfiles
````

Bare repository are special in a way that they omit working directory, therefore to use a bare repository, first we need to define the following.

- `--work-tree` - this can be your home directory, i.e., `$HOME` or `~`)
- `--git-dir` - where the repository is cloned - `$HOME/.dotfiles`

Therefore the command to use the repository will have a prefix `git --git-dir=$HOME/.dotfiles --work-tree=$HOME `, for ex -
```bash
# example - to check the logs 
git --git-dir=$HOME/.dotfiles --work-tree=$HOME log
```

For our convenience we can create an alias for this command, stick the following line (if not already present) in `.aliases` or `.zshrc`.

```bash
alias dotfiles='/usr/bin/git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME
``` 

At this point, all your configuration files are being tracked, and we can easily use the newly registered `dotfiles` command to manage the repository, ex :-

```bash
# to check the status of the tracked and untracked files 
dotfiles status

# to add a file 
dotfiles commit .tmux.conf -m ".tmux.conf added"

# push new files or changes to the github
dotfiles push origin main
```

> :warning: The `dotfiles status` will show all the untracked files to disable this behavior, do the following

```bash 
# to remove the untracked directories and files from the listing
dotfiles config --local status.showUntrackedFiles no 
```

**The great things about managing dotfiles with a git repo are**
- no prerequisite - only `git`
- we can have multiple versions of our configs using `git commits/tags`
- we can also create profiles with `git branch`.

```bash
# Manage multiple profiles - check out to the work profile 
dotfiles checkout work # or any other branch name
```

Recommended Reading: [The best way to store your dotfiles: A bare Git repository ](https://www.atlassian.com/git/tutorials/dotfiles)

#### Automated installation 



The installation of apps, libraries, and other tools are automated, open the `setup` file and update the following code according to your needs.

> if you are on other platforms then macOS, you can create similar `bash`/`batch` files and make use of `apt-get` or `winget`

```bash
# Brew casks and binaries that you need
bins = (
    "nvm"
    "php@7.4"
    "composer"
    # more valid brew formula here
)

casks=(
    "slack"
    "postman"
    # add brew casks that you need 
)
```

> :exclamation: you may need to add some post-installation code in the later part of scripts. Please see the library installation page for the steps

After updating the `setup` file, just run the file in the terminal to install:
```bash
./setup
```

:wine_glass: Voila! you are all set (in what 5 mins? cool isn't it?), Just to show you the gist of what the terminal looks

![terminal.png](/static/terminal.png)

### To start contributing to dotfiles

Suggestions / Improvements or any other helpful trick is always welcome, Please raise a PR with some context or any helpful links.