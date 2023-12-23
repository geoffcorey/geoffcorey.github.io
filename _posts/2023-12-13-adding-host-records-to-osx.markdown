---
layout: post
title:  "Adding host records to OS/X"
date:   2023-12-23 10:18:29 -0500
categories: osx dns
---
Adding Host Records is pretty straight forward.  Typically I do this when I'm running a local virtual machine or for testing services.  Slightly different then Linux because of OS/X DNS caching services.

  1. Edit /private/etc/hosts
  2. Add my.domain.name
  3. Flush the cache

```shell
sudo vi /private/etc/hosts
sudo dscacheutil -flushcache
sudo killall -HUP mDNSResponder
```
