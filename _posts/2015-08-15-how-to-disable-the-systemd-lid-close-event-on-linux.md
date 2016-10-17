---
layout: post
title: How to disable the systemd lid close event on linux
date: 2015-08-15T16:46:30+00:00
author: Geoff Corey
categories:
  - Systemd
tags:
  - 15.04
  - laptop lid
  - linux
  - systemd
  - ubuntu
---
How to disable the systemd lid close event on linux has been tough to find a working solution for me. I have a faulty lid switch on my ZaReason Ultralap 440 laptop running Ubuntu 15.04 and when I adjust the lid back it would fire off a lid close event and the machine would power off.

I changed /etc/systemd.logind.conf to ignore HandlePowerKey and HandleLidSwitch. That by itself didn&#8217;t fix the issue.

/etc/systemd/logind.conf
<pre>
# This file is part of systemd.
#
# systemd is free software; you can redistribute it and/or modify it
# under the terms of the GNU Lesser General Public License as published by
# the Free Software Foundation; either version 2.1 of the License, or
# (at your option) any later version.
#
# Entries in this file show the compile time defaults.
# You can change settings by editing this file.
# Defaults can be restored by simply deleting this file.
#
# See logind.conf(5) for details.
[Login]
#NAutoVTs=6
#ReserveVT=6
#KillUserProcesses=no
#KillOnlyUsers=
#KillExcludeUsers=root
#InhibitDelayMaxSec=5
#HandlePowerKey=poweroff
HandlePowerKey=ignore
#HandleSuspendKey=suspend
#HandleHibernateKey=hibernate
#HandleLidSwitch=suspend
HandleLidSwitch=ignore
#HandleLidSwitchDocked=ignore
#PowerKeyIgnoreInhibited=no
#SuspendKeyIgnoreInhibited=no
#HibernateKeyIgnoreInhibited=no
#LidSwitchIgnoreInhibited=yes
#HoldoffTimeoutSec=30s
#IdleAction=ignore
#IdleActionSec=30min
#RuntimeDirectorySize=10%
#RemoveIPC=yes
</pre>

I created an alias command:

<pre>
alias ignorelidswitch='systemd-inhibit --what=handle-lid-switch sleep 2592000 &
</pre>

and execute that when I startup my terminal session after logging into the system. Now under i3wm I no longer have my computer shutting down when the screen gets bumped by one of my cats or when I adjust the angle.
