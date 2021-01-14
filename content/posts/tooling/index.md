---
author: Manish Sahani
title: "Tooling is hard and necessary"
---

![banner](images/banner.jpg)

 
Something is fascinating about customizing your operating system through dotfiles, and there are tons of articles out there on what you can do with these dotfiles. These simple invisible files may seem pointless for a novice, but they become a swiss army knife if properly configured.

In this article we'll discuss configuring these files and the best way to manage them for multiple devices.

> This article will discuss a way to setup a new machine in under a minute and a seamless way to switch between two different machines using the single repository.

Dotfiles customizes your system's software in a way to maximize your productivity. There is a large dotfiles community, and with it comes a large number of customizations repositories and tons of articles on customization. In this article, first, we'll look at the best way to manage and share these dotfiles, and In the later section, we'll write an automation script to set up a new machine.

# Managing and tracking [dot]files

Almost all the developers use `git` to store and share these files and `symlinks` to sync them. Well, `symlinks` works, but it isn't the best way to sync your local files to the git repository. There is a much better solution to this, written by people at Atlassian -- [The best way to store your dotfiles: A bare Git repository ](https://www.atlassian.com/git/tutorials/dotfiles).

The trick to managing these dotfiles is by creating a [bare](https://www.atlassian.com/git/tutorials/setting-up-a-repository/git-init) git repository. If you are starting from scratch and have not tracked your dotfiles before, make a `bare` repository in the `$HOME` directory.

```bash
git init --bare $HOME/.dotfiles
```

Bare repository are special in a way that they omit working directory. Therefore to use a bare repository, first we need to define a `--work-tree` for the repository to `$HOME` directory  (since dotfiles live there). To use dotfiles repository globally we also set a `--git-dir` as `$HOME/.dotfiles`. Now the command will look something like below.
```bash
git --work-dir $HOME --git-dir $HOME/.dotfiles [command]
```

Just to make it easier to use we'll alias this to `dotfiles` which we will use instead of regular git to interact with our dotfiles repository.

```bash
alias dotfiles="/usr/bin/git --git-dir=$HOME/.dotfiles --work-tree=$HOME"
```

We also a set a flag to out local dotfiles repository - to hide all the files that we are not explicitly tracking, so that we only see files that we are interested in tracking.
```
# hides all the untracked files when status command is called
dotfiles config status.showUntrackedFiles no
```

Now we are good to track our dotfiles using the `dotfiles` command, some of the examples are: 
```bash
# to check the version history 
dotfiles log

# to check the status of the tracked and untracked files 
dotfiles status

# to add a file for tracking
dotfiles commit .vimrc -m ".vimrc added"

# push new files or changes to the github
dotfiles push origin main
```

In the later section this article, we'll see the methods to efficently config your system by creating and adding some dotfiles like - `.zshrc`, `.aliases`, etc.

# Use Cases and Advantages of dotfiles

This method of managing and sharing has various advantages some of them are listed below.

### Easy setup

Set up of a new machine can be a time consuming task, but with this method we can to use our personalized configs in under a minute. 
We just need to clone the repository and source the `.bashrc` or `.zshrc` file.

```bash
git clone --bare https://github.com/<username>/dotfiles.git $HOME/.dotfiles && source ~/.zshrc
```

### Versioned dotfiles 

Best for experimenting with new configurations and keep the change history (Basically all the pros of using git)

```bash
# to check the version history 
dotfiles log
```

### Share on Multiple devices

Share the same configs of multiple devices with minimal changes using `branch`, create a branch for your new machine, example:-

```bash
# Create configurations specific to your aws machines
dotfiles checkout -b aws
```

### Profiles for dotfiles

Create configs based on your environment using `branch`, create a branch and configure according to you work env.

```bash
# Manage multiple profiles - check out to the work profile 
dotfiles checkout work
```
