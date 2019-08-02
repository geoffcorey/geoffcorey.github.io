---
layout: post
title: Using Ansible to Create a Linux Development Environment
date: 2015-09-29T19:25:03+00:00
author: Geoff Corey
categories:
  - Dotfiles
tags:
  - ansible
  - devbox
  - docker
  - vagrant
---
# Using Ansible to Create a Linux Development Environment

Using Ansible to create a Linux development environment to develop with Node.js, GoLang, Ruby, Docker was on my list of to-dos for sometime and just moved up in priority.  I know very soon I will have to switch back from Linux to OS/X as my primary machine.   Been there before and really prefer using tools under Linux rather then OS/X.   Brew is fine and I will eventually try and set up everything native on OS/X.  Since I have the time I thought it would be a good idea to try and build a development environment using <a href="http://www.ansible.com/" target="_blank">Ansible</a> that can be spun up using Vagrant on Docker or Virtualbox.

<a href="http://www.ansible.com/" target="_blank">Ansible</a> is a provisioning system such as <a href="https://www.chef.io/chef/" target="_blank">Chef</a>, <a href="https://puppetlabs.com/" target="_blank">Puppet</a> and <a href="http://saltstack.com/" target="_blank">Salt</a>.   What is nice is I don&#8217;t have to bootstrap the box before provisioning.   So I started out looking at other folks Ansible projects on <a href="https://github.com/search?utf8=%E2%9C%93&q=devbox+ansible" target="_blank">GitHub</a> (just search for &#8220;devbox ansible&#8221;) and see how folks were making their scripts.   I decided to take one of the projects, <a href="https://github.com/samuell/devbox-golang" target="_blank">samuell/devbox-golang</a> and heavily modified it to my liking.   The result is an Vagrant script that will invoke the Ansible script to build out a Docker image or Virtualbox VM with Ubuntu 14.04 and all the development tools I typically use.

Most of the Ansible scripts are pretty simple.   I swapped out Samuell&#8217;s GoLang role for one from [Ansible Galaxy](https://galaxy.ansible.com/explore#/).  No need for reinventing the wheel.    The bulk of the customization of my environment is found in <a href="https://github.com/geoffcorey/devbox-ansible/tree/master/roles/dotfiles" target="_blank">roles/dotfiles</a>.   Here I have all my dotfiles and a script to deploy them on the <a href="https://www.vagrantup.com/" target="_blank">Vagrant</a> image.   Eventually I would like to have this setup to use vcsh and mr to have direct git repos in the home directory as I described in my blog post, &#8220;<a href="http://www.geoffcorey.com/2015/03/managing-dotfiles-across-multiple-platforms/" target="_blank">Managing Dotfile Across Multiple Platforms</a>&#8220;.   This is really handy when you update .zshrc or .nvimrc file you can commit the change and push it up to github immediately so I can pull the change on other environments. Right now the dotfiles are part of the repo.

Source can be found at <a href="https://github.com/geoffcorey/devbox-ansible" target="_blank">github.com/geoffcorey/devbox-ansible</a> If you want to fork or just use it as a starting point just note that vars/user.yml needs to exist so I can close my <a href="http://github.com/geoffcorey/bin" target="_blank">github.com/geoffcorey/bin</a> repo.   Eventually it will also be used to clone a pass repo I use on BitBucket.

Current setup can be found in the <a href="https://github.com/geoffcorey/devbox-ansible/blob/master/README.md" target="_blank">README.md</a> but the image defaults to zsh, git, mecurial, bzr, subversion, neovim with a ton of plugins, tmux, golang, nvm/node.js, rvm/ruby, python2.7, python3 and database clients for mongodb, postgresql, mysql, redis

Update 07/2019 - This method was quickly abandoned in favor of using mr, vcsh and git.
