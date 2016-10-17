---
layout: post
title: Secure cross-platform password management
date: 2015-12-26T17:59:23+00:00
author: Geoff Corey
categories:
  - Android
  - Linux
  - OS/X
  - Security
tags:
  - Android
  - cross-platform
  - linux
  - osx
  - Password Management
  - Security
  - ubuntu
---
Secure cross-platform password management is required not only because we have multiple devices but also to lock accounts by having unique passwords.  Protection from one service getting hacked so it does not compromise your other account by using the same passwords

## Password Generation

If you are writing down passwords, using the same passwords for multiple accounts or creating passwords from names and birth dates then don&#8217;t whine when your accounts get hacked.<figure style="width: 740px" class="wp-caption alignnone">

<img class="" src="http://i1.wp.com/imgs.xkcd.com/comics/password_strength.png?resize=650%2C528" alt="XKCD: Password Stength" data-recalc-dims="1" /><figcaption class="wp-caption-text">Password Strength</figcaption></figure>

To add to the security, once a year, I have an Internet Spring Cleaning and go through all my accounts and change the passwords or delete accounts I no longer use.

I use <a href="http://world.std.com/~reinhold/diceware.html" target="_blank">Diceware</a> to generate passwords via a [python script](https://pypi.python.org/pypi/diceware/0.5)

## Secure Cross-platform Password Management

### My Requirements

I use OS/X for work,  Linux at home and Android for mobile.   I am a heavy command-line user so I like a solution that I can use via command-line but also use GUI for easy management.

### The Solutions

There are many solutions suggested from [PrivacyTools.io](https://www.privacytools.io/)  (Note: I highly recommend visiting), however I am going a slightly more difficult setup then recommended.   On the list I do like how [Master Password](https://ssl.masterpasswordapp.com/) is implement and may consider switching to that solution if I didn&#8217;t have everything stored in my current solution.  I also like [SpiderOak&#8217;s Encrypr](https://spideroak.com/solutions/encryptr) but would probably still generate my own passwords.  I have used [KeePass](http://keepass.info/download.html) in the past but the cross-platform solution seemed wonky.

Update 1/11/16 &#8211; I took another look at Master Password and there are a few drawbacks.  First, all you can generate is a password. You cannot store any other information such as user name.   I suppose you could make a file for user names and urls and then use Master Password.   Second issue is some sites have length and character restrictions which Master Password does not take into account when generating a password.   Third, some sites require passwords to be changed in a period of time.   Only way to do that is change your pass phrase which would cause you to either change all your passwords or start remembering pass phrases for each site.   SpiderOak&#8217;s Encrypr looks really good and would be an easier solution then the &#8220;Nerd&#8221; solution I outlined below.   Only missing feature is a command line client.

### The solution for the Nerd

<a href="http://Pass: The standard UNIX password manager" target="_blank">Pass: The standard UNIX password manager</a> has clients for all the major OS&#8217;s, command-line autocomplete, [dmenu](http://git.zx2c4.com/password-store/tree/contrib/dmenu) integration add even Firefox (however I don&#8217;t use this integration feature).   It uses your GNU PGP key for strong encryption and can be synchronized using GIT source management tool.  I use <a href="https://qtpass.org/" target="_blank">qtpass</a> for the GUI.   There are also clients for iOS, Windows, etc.

On Android I use <a href="https://play.google.com/store/apps/details?id=org.sufficientlysecure.keychain" target="_blank">OpenKeychain: Easy PGP</a> and <a href="https://play.google.com/store/apps/details?id=com.zeapo.pwdstore" target="_blank">Password Store</a>.  I exported my <a href="https://www.gnupg.org/" target="_blank">GPG</a> key my initial setup using GPG command and imported on Android after copying the file over via USB.

To add/edit the password data I export public and private key from the initial system setup using gpg and import into the other machines.

<pre class="SCREEN"><tt class="USERINPUT"><b>gpg --armor --export-all me@myemail.com</b></tt></pre>

For Android I copy the file over via USB connection and import into via OpenKeychain and then remove the files.

I sync all my encrypted password with <a href="https://bitbucket.org/product/features" target="_blank">BitBucket.org</a> private GIT repo which is free for individuals and lock down the account with two-factor authentication using <a href="https://www.authy.com/app/mobile/" target="_blank">Authy</a>

&nbsp;
