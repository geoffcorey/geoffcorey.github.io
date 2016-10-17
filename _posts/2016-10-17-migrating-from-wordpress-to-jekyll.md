---
layout: post
title: Migrating from Wordpress to Jekyll
date: 2016-10-17T13:23:19+00:00
author: Geoff Corey
categories:
  - Blogging
tags:
  - wordpress
  - jekyll
  - github.io
---
Wordpress is nice and pain.  I use Bluehost to host Wordpress and my personal domain
email.  Bluehost has been great but I'm getting tired of Wordpress.

I decided to migrate my articles to github.io.  To make do this I installed *WordPress
to Jekyll Exporter by Ben Balter* to export my articles.   This downloads as a zip.   The really
important bits is going to be _posts directory.  WordPress editor crapped up some of the
articles I needed to be edited to make use of Jekyll features such as syntax highlighting
code.  As of time of this writing, I am still cleaning up links and images.

Next I followed github.io's instructions to setup my personal page.  I decided to use
[Lagom's Theme](https://github.com/swanson/lagom) which is a nice minimal theme.

After installation, copying _posts over and editing them to add syntax highlighting I
was ready to redirect the old articles to the new.  I used another plugin
*Redirection by John Goley* to 301 permanment redirect the old url to the new url. I
don't have a lot of popular articles but
[Setup Amazon AWS wildcard SSL certificate](https://geoffcorey.github.io/amazon%20aws/ssl/2015/04/19/setup-amazon-aws-wildcard-ssl-certificate.html)
is very popular and I would hate for search engines to direct people to a dead link when
I finally shutdown my WordPress site.
