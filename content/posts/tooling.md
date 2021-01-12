---
author: "Manish Sahani"
title: "Tooling is hard and necessary"
---

<!-- ![banner.jpg](/images/dotfiles.jpg) -->

There is something fascinating about customizing your operating system through dotfiles, and there are tons of articles on what you can do with these dotfiles. These simple invisible files may seem pointless for a novice, but they become a swiss army knife if properly configured.

This repository is the collection of configurations that I learned over time and still use for my daily work. I primarily use a 13' Macbook pro (named hades) and Ubuntu on AWS/GCP; therefore, this repository mainly applies to macOS and debians, but Improvements or contributions for other platforms are more than welcome. 

This document mainly talks about a cool way to manage and share dotfiles across multiple surfaces (mac, ubuntu etc), Please star the repository if you find it useful.

## Managing and tracking [dot]files

If you are new to this, you may want to read the article by atlassian. - [The best way to store your dotfiles: A bare Git repository ](https://www.atlassian.com/git/tutorials/dotfiles).

The trick to managing these dotfiles is by creating a [bare](https://www.atlassian.com/git/tutorials/setting-up-a-repository/git-init) git repository. So, If you want to start from zero configs follow the above article, otherwise if you want to use these dotfiles (**recommended**) follow the following steps.

### To start using these dotfiles

First fork the repository, **review the files and code**, and **remove code that you don't need**. :warning: Don't blindly use these settings unless you know what that entails.

```bash
# after forking replace the <username> with your github handle
git clone --bare https://github.com/<username>/dotfiles.git $HOME/.dotfiles
````

> Notice the `--bare` flag, this the clones repository as a bare repository. Bare repository are special in a way that they omit working directory, therefore to use a bare repository, first we need to define the following.
> - `--work-tree` - this can be your home directory, i.e., `$HOME` or `~`)
> - `--git-dir` - where the repository is cloned - `$HOME/.dotfiles`
>
> Therefore the command to use the repository will have a prefix `git --git-dir=$HOME/.dotfiles --work-tree=$HOME `, 

Now next step is to checkout to the proper branch according to the os of the machine.
```bash
# checkout to main for macos and linux for ubuntu.
git --git-dir=$HOME/.dotfiles --work-tree=$HOME checkout main
```

If you have a new machine or wants to install all your apps and libs, use the `setup` script to automate the installation - see the [Automated installation](#automated-installation) section for more info. Run the setup script 
```bash
# this will install all your apps
bash ~/setup
```

Finally source the `.zshrc` or `.bashrc` by doing `source ~/.zshrc` and Voila! Thats it. You did it. 

> Yes all it takes is 3 steps to configure you new machine, Swiss army knife!


### Usage of dotfiles

This method of managing and sharing has various advantages some of them are shown below:

**1. Keep the dofitles versioned, (basically all the cons of using git)**
At this point, all your configuration files are being tracked, and you can easily use the `dotfiles` command ([see this line in .aliases](https://github.com/kalkayan/dotfiles/blob/main/.aliases#L69)) to manage the repository, some examples are:-
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

> :warning: The `dotfiles status` will show all the untracked files to disable this behavior, do the following
>
> ```bash 
> # to remove the untracked directories and files from the listing
> dotfiles config --local status.showUntrackedFiles no 
> ```

**2. Share on Multiple devices**
Share the same configs of multiple devices with minimal changes using `branch`, create a branch for your new machine, example:-

```bash
# Create configurations specific to your aws machines
dotfiles checkout -b aws
```

**3. Create Profiles for dotfiles**
Create configs based on your environment using `branch`, create a branch and configure according to you work env.
```bash
# Manage multiple profiles - check out to the work profile 
dotfiles checkout work
```

## Automated installation 

The repository comes with a bash script (`setup`) to automate the installation of all the binaries and applications. Depending on the platform the setup file will changes for example - for macos it uses `brew` and for ubuntu it uses `snap` or `apt-get`. In any case open the `setup` file and update the code according to your needs.

### Macos 

Just add the bins to the `bins` array and brew `casks` to the `casks` array, the script will do all the installation with proper checking.
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

You may need to add some post-installation code in the later part of scripts. Please see the library installation page for the steps

### Linux Setup

Currently, the automated setup is not available for linux machine but the idea is the same.

```bash
##############################################################################
#                   /~\ These are my things you may skip /~\                 #
##############################################################################

# Install basic pre-requisites 
sudo apt-get install libgl1-mesa-glx libegl1-mesa libxrandr2 libxrandr2 libxss1 libxcursor1 libxcomposite1 libasound2 libxi6 libxtst6

# Install nvim 4.4 (the apt-get neovim doesn't work with some pluggins)
sudo snap install nvim --classic

# Install Plug
sh -c 'curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs \
       https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'

# Install Fzf (also do the fzf post-installation stuff)
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf &&  ~/.fzf/install

# Install node (node with snap creates problem with coc-vim so going with the classical way)
curl -sL https://deb.nodesource.com/setup_12.x -o nodesource_setup.sh # replace 12 with Version.
bash nodesource_setup.sh && sudo apt-get install nodejs

# Install Yarn (because why not)
curl -o- -L https://yarnpkg.com/install.sh | bash
```

## Contributing to dotfiles

Suggestions / Improvements or any other helpful trick is always welcome, Please raise a PR with some context or any helpful links.

Feel free to reach me out at [rec.manish.sahani@gmail.com](mailto:rec.manish.sahani@gmail.com) or connect with me on [LinkedIn](https://www.linkedin.com/in/manishsahani).