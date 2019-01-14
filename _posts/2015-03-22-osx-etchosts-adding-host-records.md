---
layout: post
title: 'OS/X /etc/hosts &#8211; Adding Host Records'
date: 2015-03-22T13:34:13+00:00
author: Geoff Corey
categories:
  - OS/X
tags:
  - dns
  - etc
  - hosts
  - osx
---
[<img class=" size-thumbnail wp-image-184 alignright" src="http://i2.wp.com/www.geoffcorey.com/wp-content/uploads/2015/03/os-x-mavericks-logo.png?resize=150%2C150" alt="OS/X Mavericks Logo" data-recalc-dims="1" />](http://i2.wp.com/www.geoffcorey.com/wp-content/uploads/2015/03/os-x-mavericks-logo.png) OS/X /etc/hosts &#8211; Adding Host Records is pretty straight forward.  Typically I do this when I&#8217;m running a local virtual machine or for testing services.  Slightly different then Linux because of OS/X DNS caching services.

  1. Edit /private/etc/hosts
  2. Add my.domain.name
  3. Flush the cache

{% highlight shell %}
sudo vi /private/etc/hosts
sudo dscacheutil -flushcache
sudo killall -HUP mDNSResponder
{% endhighlight %}
