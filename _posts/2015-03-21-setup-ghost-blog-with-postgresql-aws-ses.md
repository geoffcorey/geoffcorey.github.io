---
id: 145
title: Setup Ghost Blog with Postgresql / AWS SES
date: 2015-03-21T13:23:43+00:00
author: Geoff Corey
layout: post
guid: http://www.geoffcorey.com/?p=145
permalink: /2015/03/setup-ghost-blog-with-postgresql-aws-ses/
post_icons:
  - 0
featured_stamp:
  - 0
slider_images:
  - 0
image:
  - 
link_title:
  - 
link_url:
  - 
the_quote:
  - 
video_height:
  - 280px
video_upload:
  - 
video_url:
  - 
video_poster:
  - 
video_embed:
  - 
audio_upload:
  - 
audio_url:
  - 
categories:
  - Node.js
tags:
  - Amazon SES
  - blog
  - ghost
  - node.js
  - postgresql
---
<figure id="attachment_150" style="width: 180px" class="wp-caption alignright">[<img class="wp-image-150 size-full" src="http://i0.wp.com/www.geoffcorey.com/wp-content/uploads/2015/03/amazon-ses-180x180-e1448419526850.png?fit=100%2C100" alt="amazon-ses-180x180" data-recalc-dims="1" />](http://i2.wp.com/www.geoffcorey.com/wp-content/uploads/2015/03/amazon-ses-180x180.png)<figcaption class="wp-caption-text">Amazon SES</figcaption></figure> 

Configuring <a title="Ghost blogging platform" href="http://ghost.org" target="_blank">Ghost</a> Blog for <a title="Postgresql Open Source Database" href="http://postgresql.org" target="_blank">Postgresql</a> and <a title="Amazon Web Services - SES" href="https://console.aws.amazon.com/ses/home" target="_blank">Amazon SES</a> for mail took a lot longer then it should.   Ghost is a blogging platform written in <a title="Node.js" href="http://nodejs.org" target="_blank">Node.js</a>. Docs are a bit underwhelming for self hosting and I am not sure if that is on purpose to sign up for their hosted solution or nobody seems to care.

After searching a lot of pages that were not on Ghost&#8217;s site I finally had a solution that works.   I hope this helps a lot of other folks not waste a lot of time hosting their own solution.

# Setup Ghost Blog with Postgresql / AWS SES

On Amazon Web Service (AWS) setup a new <a title="Amazon Web Services - IAM" href="https://console.aws.amazon.com/iam/home" target="_blank">Identy Access Management (IAM) </a>user and download the credentials for AWS access keys. Add a policy for that user to be able to use Email Sending Service (SES). Go to SES and create a verified email used to send email. In this example I am using no-reply@mycompany.com.

Environment variables:

<pre>export APP_URL=http://blog.mycompany.com
export VERIFIED_FROM_EMAIL=no-reply@mycompany.com
export AWS_ACCESS_KEY_ID=
export AWS_SECRET_KEY=
export DB_URL=postgres://&lt;username&gt;:&lt;password&gt;@&lt;host&gt;:&lt;port&gt;/&lt;dbname&gt;
</pre>

Here is the snippet of the config.js that will allow you to use Postgresql and Amazon SES.

<pre>var config;

config = {
    // ### Production
    // When running Ghost in the wild, use the production environment
    // Configure your URL and mail settings here
    production:
    {
    url: process.env.APP_URL,
    mail:
    {
      transport: 'SES',
      fromaddress: process.env.VERIFED_FROM_EMAIL,
      options:
      {
        AWSAccessKeyID: process.env.AWS_ACCESS_KEY_ID,
        AWSSecretKey: process.env.AWS_SECRET_KEY
      },
    },
    database:
    {
      client: 'postgres',
      connection: process.env.DB_URL,
      debug: false
    },
    server:
    {
      host: '0.0.0.0',
      port: process.env.PORT
    }
  }
};

// Export config
module.exports = config;
</pre>

<div class="changetip_tipme_button" data-bid="pzorhLpQgQWHugNp82hjHF" data-uid="kZJeSKkyNFLTcR9hhZcRyH">
</div>