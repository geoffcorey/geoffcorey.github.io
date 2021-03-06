---
layout: post
title: Rethinking my Linux setup
date: 2015-12-29T02:57:06+00:00
author: Geoff Corey
categories:
  - Linux
tags:
  - linux
---
I have been rethinking my Linux setup.   Several things I like and don&#8217;t like with my current setup.

## Current Linux setup

  * Distro: Ubuntu 16.04 (Home) RHEL 7 (work)
  * Window Manager: i3 + i3bar + conky + hack font
  * Dotfiles Management: vcsh, mr, git
  * Password Management: [Pass](http://www.passwordstore.org/), git, [GnuPG](https://www.gnupg.org/)
  * Bookmark Management: Pocket
  * Email: GMail, ProtonMail, domain hosted + mutt + [GnuPG](https://www.gnupg.org/)
  * Encryption:[GnuPG](https://www.gnupg.org/), [keybase.io](https://keybase.io/geoffcorey)
  * Cloud Storage: SpiderOakOne, Google Drive, Dropbox and encrypting before syncing
  * IRC: WeeChat
  * Messaging: Signal, Hangouts

##

<img src="/images/2016-10-25-210749_1920x1080_scrot.png" height="270" width="480" />

## What I like and don&#8217;t like?

I am a heavy terminal user so I&#8217;m good with the window manager, IRC client and encryption.

Password management setup is a little complex.  There is always the chance that someone gets a
encrypted copy of my keys and my GPG keys and start up some password cracker.   Good luck with 
that if you want to try.   I am interested in trying out 
[Master Password](https://ssl.masterpasswordapp.com/).  It has cross platform clients and a 
command-line client as well.

**UPDATE 01/03/16:** [Master Password](http://masterpasswordapp.com/) is buggy.  Tried the compile 
the CLI and tried the Java version.   CLI cannot compile and zero instructions on what is required.   
The Java GUI client crashes when adding a web site.   So I will stay with my 
[current setup](http://geoffcorey.github.io/2015/12/secure-cross-platform-password-management/).

**UPDATE 01/31/16:** Been using my old setup <a href="http://www.passwordstore.org" target="_blank">
pass: the standard unix password manager</a> along side with <a href="https://spideroak.com/solutions/encryptr" 
target="_blank">Encryptr</a>.  Both are open-source and work on multiple platforms.   Encryptr is a lot easier 
to setup then Pass.   Only thing missing from Encryptr is a command-line tool but that is not a deal breaker.

**UPDATE 10/25/16:** Going back to [Master Password](http://masterpasswordapp.com/) using web app and Android 
for most passwords.  Others will still be Encryptr.

**UPDATE 02/01/19:** Using [BitWarden](https://bitwarden.org) now for password management.

I think I&#8217;m going back to Firefox.   Chrome has crashed a few too many times due to some race condition. 
I suppose I could use Firefox bookmarks and sync across platforms that way.  In general I have been backing off 
on Google tools.  GMail/Inbox/Google Now has me a bit freaked out.    Been using [Signal](https://whispersystems.org/) 
more then Hangouts recently and I got into the desktop beta as well for Signal. [DuckDuckGo](https://duckduckgo.com/) 
instead of Google Search but sometimes I drop back.   I still think Google generates better results, especially when 
I time box the results.   Still learning all the DuckDuckGo [!Bangs](https://duckduckgo.com/bang) and I can see the 
DuckDuckGo advantages.

For cloud storage I have pretty much moved entirely over to Spider Oak One.  Dropbox isn&#8217;t running on my machine 
anymore and Google Drive still has some Google Docs files but most everything else is in Spider Oak One now.  My most 
sensitive documents are encrypted with [GnuPG](https://www.gnupg.org/) prior to storing on Spider Oak as a belt and 
suspenders procedure.

WeeChat still makes me smile so I am good with it being my main IRC client.

Dotfile management,  I don&#8217;t know.   It works well enough but when you go back and forth from Ubuntu and OS/X 
and sometime a  Red Hat flavor distro it take great care to accommodate the distros in the dotfiles.   Because I 
update the dotfiles on all much installs this solution with [vcsh](https://github.com/RichiH/vcsh), 
[mr](https://github.com/joeyh/myrepos) and git works well and I don&#8217;t have to do symlinks in the home directory.
I can also piece mail what is installed.   For instance if I am working on a server with no gui, I can install the vim 
and zsh repo and be happy.  I have to start adding uname tests in my .zshrc and .aliases scripts to properly set some 
things from OS/X and Linux.

So the big question is do I stick with Ubuntu?  I have used Gentoo, Arch, SUSE, Fedora in the past.   In general I am 
not a fan of RPM flavor distros.   Gentoo is fine but compiling everything is a time sync.   So Arch or Ubuntu?   
Ubuntu is quick and easy to install.   If I do a reinstall I will likely do disk encryption but I need to read 
up more on issues that can plaque laptops like suspend or sudden power offs if the encrypted disk holds up.  I would 
consider FreeBSD as well but I do a lot of Docker development.
