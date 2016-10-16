---
id: 281
title: OpenStack OS::Heat::CloudConfig OS::Heat::MultipartMime
date: 2016-03-01T18:17:56+00:00
author: Geoff Corey
layout: post
guid: http://www.geoffcorey.com/?p=281
permalink: /2016/03/openstack-heat-osheatmultimim/
categories:
  - OpenStack
tags:
  - openstack
---
<div id="dwa_widget_label_0" class="idw-label pim-mailread-mailcontent" data-dojo-type="dwa.widget.label" data-dojo-attach-point="bodyContent" data-dojo-props="formatter: socmail.common.BodyHtmlFormatter, constraints: { inlineAttachments: this.model.inlineAttachments }, rawHtml: true">
  <p>
     <a href="http://i0.wp.com/www.geoffcorey.com/wp-content/uploads/2016/03/3227890.jpg" rel="attachment wp-att-291"><img class="alignleft wp-image-291 size-medium" src="http://i0.wp.com/www.geoffcorey.com/wp-content/uploads/2016/03/3227890-300x222.jpg?fit=300%2C222" alt="Openstack::Heat::CloudConfig with Openstack::Heat::MultipartMime without setting merge_how is a programming trap." srcset="http://i0.wp.com/www.geoffcorey.com/wp-content/uploads/2016/03/3227890.jpg?resize=300%2C222 300w, http://i0.wp.com/www.geoffcorey.com/wp-content/uploads/2016/03/3227890.jpg?w=491 491w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" /></a>Debugging OpenStack Heat Template with OS:Heat:CloudConfig resources in OS::Heat::MultipartMime where only the last config was executed.   This was pretty frustrating to figure out what was happening.   If I rearranged the order, only the last config would execute.  It was clear that expected behavior was a trap.  Using an <a href="http://example from OpenStack Heat Templates" target="_blank">example from OpenStack Heat Templates</a> to illustrate proper usage I can illustrate what was happening and what to fix.
  </p>
</div>

<div dir="ltr">
  I have seen other folks use OS::Heat::SoftwareConfig and then echo their settings to a file to avoid this programming pitfall.   The result is less readable, messy code.   I knew I was missing something and after a lot of reading and studying templates I found the solution.
</div>

<div id="dwa_widget_label_0" class="idw-label pim-mailread-mailcontent" data-dojo-type="dwa.widget.label" data-dojo-attach-point="bodyContent" data-dojo-props="formatter: socmail.common.BodyHtmlFormatter, constraints: { inlineAttachments: this.model.inlineAttachments }, rawHtml: true">
  <div dir="ltr">
  </div>
  
  <h2 dir="ltr">
    How to make sure all the OS::Heat::CloudConfig resources are executed using OS::Heat::MultipartMime
  </h2>
  
  <div dir="ltr">
    Using an <a href="http://example from OpenStack Heat Templates" target="_blank">example from OpenStack Heat Templates</a> to illustrate proper usage I can illustrate what was happening and what to fix.
  </div>
  
  <div dir="ltr">
  </div>
  
  <div dir="ltr">
    Example snippet:
  </div>
  
  <pre dir="ltr">five_init:
  # this resource demonstrates multiple cloud-config resources
  # with a merge_how strategy
  type: OS::Heat::CloudConfig
  properties:
    cloud_config:
      <strong>merge_how: 'dict(recurse_array,no_replace)+list(append)'</strong>
      write_files:
      - path: /tmp/five
      content: |
       The five is bar
       look what happens if you take out the merge_how
       and you will see that all the configs in 
       server_init do not execute
</pre>
  
  <pre dir="ltr">server_init:
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
  
  <div dir="ltr">
  </div>
  
  <div dir="ltr">
     The key is the &#8220;merge_how&#8221; setting in OS::Heat::CloudConfig that makes this work, otherwise the new &#8220;- config&#8221; would replace the previous OS::Heat::CloudConfig resource.    By adding the &#8220;merge_how&#8221; setting we can override this behavior and append the setting.   For each of the configs in the OS::Heat::MultipartMime I had to make sure &#8220;merge_how&#8221; was added.  You can read more about merge_how in the <a href="http://cloudinit.readthedocs.org/en/latest/topics/merging.html" target="_blank">cloud_init docs</a>
  </div>
  
  <div dir="ltr">
  </div>
</div>