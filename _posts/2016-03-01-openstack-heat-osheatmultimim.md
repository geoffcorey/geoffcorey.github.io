---
title: OpenStack OS::Heat::CloudConfig OS::Heat::MultipartMime
date: 2016-03-01T18:17:56+00:00
author: Geoff Corey
categories:
  - OpenStack
tags:
  - openstack
---
<img  src="http://i0.wp.com/www.geoffcorey.com/wp-content/uploads/2016/03/3227890-300x222.jpg?fit=300%2C222"/>

Debugging OpenStack Heat Template with OS:Heat:CloudConfig resources in OS::Heat::MultipartMime where only the last config was executed.   This was pretty frustrating to figure out what was happening.   If I rearranged the order, only the last config would execute.  It was clear that expected behavior was a trap.  Using an <a href="http://example from OpenStack Heat Templates" target="_blank">example from OpenStack Heat Templates</a> to illustrate proper usage I can illustrate what was happening and what to fix.

I have seen other folks use OS::Heat::SoftwareConfig and then echo their settings to a file to avoid this programming pitfall.   The result is less readable, messy code.   I knew I was missing something and after a lot of reading and studying templates I found the solution.


## How to make sure all the OS::Heat::CloudConfig resources are executed using OS::Heat::MultipartMime

Using an <a href="http://example from OpenStack Heat Templates" target="_blank">example from OpenStack Heat Templates</a> to illustrate proper usage I can illustrate what was happening and what to fix.

Example snippet:
<pre>
five_init:
  # this resource demonstrates multiple cloud-config resources
  # with a merge_how strategy
  type: OS::Heat::CloudConfig
  properties:
    cloud_config:
      merge_how: 'dict(recurse_array,no_replace)+list(append)'
      write_files:
      - path: /tmp/five
      content: |
       The five is bar
       look what happens if you take out the merge_how
       and you will see that all the configs in
       server_init do not execute
</pre>

<pre>
server_init:
  type: OS::Heat::MultipartMime
  properties:
  parts:
  - config: {get_resource: one_init}
  - config: {get_resource: two_init}
  # referencing another OS::Heat::MultipartMime resource will result
  # in each part of that resource being appended to this one.
  - config: {get_resource: three_four_init}
  type: multipart
  - config: {get_resource: five_init}</pre>

The key is the &#8220;merge_how&#8221; setting in OS::Heat::CloudConfig that makes this work, otherwise the new &#8220;- config&#8221; would replace the previous OS::Heat::CloudConfig resource.    By adding the &#8220;merge_how&#8221; setting we can override this behavior and append the setting.   For each of the configs in the OS::Heat::MultipartMime I had to make sure &#8220;merge_how&#8221; was added.  You can read more about merge_how in the <a href="http://cloudinit.readthedocs.org/en/latest/topics/merging.html" target="_blank">cloud_init docs</a>
