---
layout: post
title: Managing Dotfiles Across Multiple Platforms
date: 2015-03-15T21:34:37+00:00
author: Geoff Corey
categories:
  - Dotfiles
tags:
  - dotfiles
  - git
  - github
  - mr
  - myrepos
  - vcsh
---
[<img class="alignright wp-image-106 size-full" src="http://i0.wp.com/www.geoffcorey.com/wp-content/uploads/2015/03/dofile.png?fit=134%2C82" alt="managing dotfiles" data-recalc-dims="1" />](http://i0.wp.com/www.geoffcorey.com/wp-content/uploads/2015/03/dofile.png)

Managing dotfiles can be a task.  Managing dotfiles across multiple platforms need more then just a single git repo and a shell script.   I currently develop on OS/X, Linux w/i3 window manager and remote linux accounts without X Windows.   The task is compounded that I have not standardized on a single linux distribution and use both Ubuntu and Arch.

Researching on ways to manage beyond writing a custom shell script I have found using git plus vcsh and mr to fit the bill to allow me to manage dotfiles for linux, os/x and even X11 scripts.   Keeping repos separate I can manage dotfiles for the specific.

## Managing dotfiles across multiple platforms

First thing first, create the repo. If you do not have an account with github then get one. Github.com has won (see GoogleCode.com is shutting down). Next create a new repository. You can select a .gitignore file prebuilt and license. For now I&#8217;m not going to select a .gitignore file and use the MIT License. Confused on licenses? Check out this helpful page on <a title="Choosing an OSS license doesn’t need to be scary" href="http://choosealicense.com/" target="_blank">open source licenses</a>.

If this is a new github account then generate your ssh key if you don&#8217;t have one and copy/paste your key in directory ~/.ssh/id_rsa.pub to your <a title="GitHub: Manage your SSH keys" href="https://github.com/settings/ssh" target="_blank">Github account settings to manage sshkeys</a>.    If you need to generate your SSH keys:

cd ~

ssh-keygen -t rsa

Now we need to decide before doing anything how we want to install and manage the dotfiles. There are several options:

  * Soft link (ln -s)
  * Rsync
  * Copy
  * <a title="rc file (dotfile) management" href="https://github.com/thoughtbot/rcm" target="_blank">RCM</a>
  * <a title="config manager based on Git" href="https://github.com/RichiH/vcsh" target="_blank">vcsh</a>

I&#8217;m not a fan of using rsync or file copies for dotfiles because any changes would require the file be copied back to the github repo to be commited.   That seems like a good chance of me forgetting to check in changes.

Soft linking (ln -s <source> <dest>) will allow me to edit my files in my home directory and when everything is working to go back to the original files in my github project directory and commit and push.

If your a ruby developer you may like <a title="rc file (dotfile) management" href="https://github.com/thoughtbot/rcm" target="_blank">rcm by thoughtbot</a>.   The advantages is it allow for selective dotfile installations so if I don&#8217;t want to install X11 files or use Bash instead of ZSH I can do the installation with tags.   It also soft links the files so I can easily commit changes.   Only disadvantage is it depends on Ruby.  When using Ruby I prefer to use <a title="Ruby Version Manager" href="https://rvm.io/rvm/install" target="_blank">rvm</a> aka Ruby Version Manager and trying install ruby in your userspace but there are a lot of dependencies that will likely force you back to needing sudo.

For my dotfile management I am going to use vcsh.  vcsh allows you to manage multiple dotfile repos.   For me I have remote machines where I only need terminal based dotfiles such as vim, zsh, tmux, git, etc.   On my laptop I am running window manager i3, conky, dmenu, etc.   I also have a MacBook Pro running OS/X and may want to have a repo specialized for OS/X apps like iTerm.

## Bootstrapping Plan

So with all these systems there are different package managers and dependencies so let&#8217;s just create a dotfiles repo just to manage bootstrapping a new machine with dependencies.

I like to tailor my bootstrap file to the system I am installing on so I can create a bootstrap for ubuntu, OS/X, Amazon Linux, Arch, etc. Maybe we can do this in one file? Let&#8217;s try!

So right now I am using Ubuntu 14.04 and I created a ~/bin directory with bootstrap.sh.    I put in all the hooks to expand the installation for other *nix distros but for now lets focus on Ubuntu.

For development I want to install all the clients for Redis, MongoDB and MySQL which seem to be used from project to project.   I added options to install the server for those packages locally using a double-dash option switch.   I&#8217;ve been a long time Vi user and somewhere in the late 90&#8217;s switched to Vim (Vi improved).   Well now that I am redoing all my dot files it seems time to upgrade again with NeoVim.    NeoVim is a compatible with Vim but the underlying code base is not <a title="Why NeoVim is better then Vim" href="http://geoff.greer.fm/2015/01/15/why-neovim-is-better-than-vim/" target="_blank">insane as Vim</a> and it is much faster.

Here is the base packages being installed:

  * neovim &#8211; A better Vim editor
  * git &#8211; Version control
  * zsh &#8211; Z Shell
  * curl &#8211; Swiss army knife for all things HTTP
  * build-essential &#8211; Development dependencies
  * libssl-dev &#8211; Development dependencies
  * ssh &#8211; OpenSSH
  * python-pip &#8211; Needed for YouCompleteMe Vim Plugin
  * silversearcher-ag &#8211; Faster search then Grep
  * cmake &#8211; Needed for YouCompleteMe
  * python2.7-dev &#8211; Needed for YouCompleteMe Vim Plugin
  * figlet &#8211; Because it is fun
  * weechat &#8211; IRC client
  * wget &#8211; Quick and dirty Curl
  * mongodb-clients &#8211; Connect to MongoDB
  * redis-tools &#8211; Connect to Redis
  * mysql-client &#8211; Connect to MySQL
  * ctags &#8211; Source tagging
  * vcsh &#8211; config manager based on Git
  * mr &#8211; Multiple Repository management tool

Optionally using flags I can install

  * <a title="GoLang" href="https://golang.org/" target="_blank">Golang</a>
  * Node.js with <a title="Node Version Manager" href="https://github.com/creationix/nvm" target="_blank">NVM</a>
  * Ruby with <a title="Ruby Version Manager" href="https://rvm.io/" target="_blank">RVM</a>
  * <a title="Amazon Command Line Interface" href="http://aws.amazon.com/cli/" target="_blank">AWS Command Line Interface</a>

I created a clean account using RamNode where I have a remote development setup and created bin/bootstrap.sh.  After a lot of testing and tweaking I am satisfied that the script is ready to be committed to GitHub.    Now my old dotfiles repo had everything in their and while it worked it was becoming a bit messy which is why I&#8217;m starting from scratch to rebuild my development dotfiles.   This time I plan on having separate repos for specific dotfiles using vcsh.

## Managing Multiple Git Repositories in the Same Directory

From the vcsh README.md:

> vcsh allows you to maintain several Git repositories in one single directory. They all maintain their working trees without clobbering each other or interfering otherwise. By default, all Git repositories maintained via vcsh store the actual files in $HOME but you can override this setting if you want to.

So the plan is to have a repo for bin, zsh, nvim from the root of my home directory.    First thing to do is create an empty remote repo on Github.   I went ahead and created a bin directory with MIT License and in hindsight I should have created an empty repo because the license file gets written in my $HOME directory.  So I corrected that by moving the license to the specific directory for the dotfile.

https://github.com/geoffcorey/bin

Next, on my local machine we use vcsh to create a local repo for ~/bin and then push it to the remote repo on GitHub.

'''bash
  geoff@dev:~$ vcsh init bin
  Initialized empty Git repository in /home/geoff/.config/vcsh/repo.d/bin.git/
  geoff@dev:~$ vcsh bin add bin/bootstrap.sh
  geoff@dev:~$ git config --global user.email "geoff@geoffcorey.com"
  geoff@dev:~$ git config --global user.name "Geoff Corey"
  geoff@dev:~$ vcsh bin commit -m "initial commit"
  [master (root-commit) 1ba2056] initial commit
   1 file changed, 130 insertions(+)
   create mode 100755 bin/bootstrap.sh
  geoff@dev:~$ vcsh bin remote add origin git@github.com:geoffcorey/bin.git
  geoff@dev:~$ vcsh bin pull origin master
  From github.com:geoffcorey/bin
   * branch master -&gt; FETCH_HEAD
  Merge made by the 'recursive' strategy.
   LICENSE | 22 ++++++++++++++++++++++
   1 file changed, 22 insertions(+)
   create mode 100644 LICENSE
  geoff@dev:~$ vcsh bin push origin master
  Counting objects: 7, done.
  Delta compression using up to 2 threads.
  Compressing objects: 100% (4/4), done.
  Writing objects: 100% (6/6), 1.75 KiB | 0 bytes/s, done.
  Total 6 (delta 0), reused 0 (delta 0)
  To git@github.com:geoffcorey/bin.git
   d3cc2f4..3ecf045 master -&gt; master
  geoff@dev:~$ <strong>vcsh bin mv LICENSE bin
  geoff@dev:~$ <strong>vcsh bin commit -m "move LICENSE file to directory"
  [master 6bbc29d] move LICENSE file to directory
   1 file changed, 0 insertions(+), 0 deletions(-)
   rename LICENSE =&gt; bin/LICENSE (100%)
  geoff@dev:~$ vcsh bin push origin master
  Counting objects: 5, done.
  Delta compression using up to 2 threads.
  Compressing objects: 100% (2/2), done.
  Writing objects: 100% (3/3), 328 bytes | 0 bytes/s, done.
  Total 3 (delta 0), reused 0 (delta 0)
  To git@github.com:geoffcorey/bin.git
     3ecf045..6bbc29d  master -&gt; master
'''

Now that I have <a title="~/bin files" href="http://github.com/geoffcorey/bin" target="_blank">github.com/geoffcorey/bin</a> I will do the same steps for my NeoVim configs <a title="dotfiles for NeoVim" href="http://github.com/geoffcorey/neovimrc" target="_blank">github.com/geoffcorey/neovimrc</a>

## Multiple Repository Management Tool

This handle utility will allow us to quickly deploy multiple repositories.   This is a nice compliment to vcsh to manage our dotfile configurations.

### Setup mr

Lets use mr to configure the bin and vimrc repo so we can quickly deploy on a new user account.   We will start by cloning vcsh mr template.

'''bash
$ cd ~<br /> $ vcsh clone git@github.com:RichiH/vcsh_mr_template.git mr
'''

You now have

*  ~/.config/mr/available.d/mr.vcsh
*  ~/.config/mr/available.d/zsh.vcsh
*  ~/.config/mr/config.d/mr.vcsh

Next create a blank mr repo on Github and change the origin URL for mr.

'''bash
  $ vcsh mr remote set-url origin git@github.com:geoffcorey/mr.git
'''

Next let&#8217;s remove RichiH&#8217;s zsh.vcsh since we don&#8217;t plan on using his config. We also need to edit .config/available.d/mr.vcsh and point that to our repo instead of RichH&#8217;s.

'''bash
  $ nvim .config/mr/available.d/mr.vcsh
  $ vcsh mr remove .config/mr/available.d/zsh.vcsh
  $ vcsh mr commit -m "remove RichiH's zsh.vcsh and modified mr.vcsh to point to our repo"
'''

Next, create <a href="https://github.com/geoffcorey/mr/blob/master/.config/mr/available.d/bin.vcsh" target="_blank">.config/available.d/bin.vcsh</a> and <a href="https://github.com/geoffcorey/mr/blob/master/.config/mr/available.d/neovimrc.vcsh" target="_blank">.config/availble.d/neovimrc.vcsh</a> files and soft link to the ~/.config/mr/config.d directory add them to mr repo.

'''bash
  $ cd .config/mr/config.d
  $ ln -s ../available.d/bin.vcsh .
  $ ln -s ../available.d/neovimrc.vcsh .
  $ vcsh mr add .config/mr/available.d/bin.vcsh
  $ vcsh mr add .config/mr/available.d/neovimrc.vcsh
  $ vcsh mr add .config/mr/config.d/bin.vcsh
  $ vcsh mr add .config/mr/config.d/neovimrc.vcsh
  $ vcsh mr commit -m "Added bin and neovimrc"
'''

## Installing a New Account

Installing dotfiles on a new machine is now a lot easier. Just make sure you have git, vcsh and mr installed then execute the following commands.

'''bash
  $ cd ~
  $ vcsh clone git@github.com:geoffcorey/mr.git mr
  $ mr up
'''

Then you can use the bootstrap.sh to finish your installation

'''bash
  $ ~/bin/bootstrap.sh
'''

If you need to update your dotfiles from github just

'''bash
  $ cd ~
  $ mr up
'''

## Resources

<a title="Manage multiple git repos from single directory" href="https://github.com/RichiH/vcsh" target="_blank">vcsh</a>

<a title="myrepos" href="https://github.com/joeyh/myrepos" target="_blank">mr</a>

<a title="Your unofficial guide to dotfiles on GitHub." href="http://dotfiles.github.io" target="_blank">Github Dotfiles</a>

<a title="NeoVim - Better then Vim" href="http://neovim.org/" target="_blank">NeoVim</a>
