---
title: AWS Elastic Beanstalk Node.js Application Not Starting
date: 2015-03-13T03:42:52+00:00
author: Geoff Corey
categories:
  - Node.js
tags:
  - aws
  - elasticbeanstalk
  - node.js
---
I had an  AWS Elastic Beanstalk node.js application not starting in one environment but would in the other environments. I typically have 3 or more environments for a service setup: localhost, dev, staging and prod. I deploy a build using <git commit #>.zip to dev  after testing on my local machine.  If all seems well I deploy up the environment chain until it is finally deployed in production.<figure id="attachment_70" style="width: 300px" class="wp-caption alignright">

[<img class="size-medium wp-image-70" src="http://i1.wp.com/www.geoffcorey.com/wp-content/uploads/2015/03/works-on-my-machine-300x196.jpg?fit=300%2C196" alt="it works on my machine" data-recalc-dims="1" />](http://i2.wp.com/www.geoffcorey.com/wp-content/uploads/2015/03/works-on-my-machine-e1426944703435.jpg)

## Problem: AWS Elastic Beanstalk Node.js Application Not Starting

The build worked on my machine and one other environment but not the next one in the progression? How can this happen? Well I&#8217;m here to tell you . . .

## Problem: Deployment is not a clean directory with AWS Elastic Beanstalk

<a title="Deploying AWS Elastic Beanstalk Applications in Node.js Using EB CLI and Git" href="http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_nodejs.html" target="_blank">AWS Elastic Beanstalk</a> does not wipe out the directory when you load a new zip bundled <a title="node.js org" href="http://nodejs.org" target="_blank">node.js</a> zip file to deploy. It simply overwrites the directory with whatever is unpacked.

## Problem: Dependencies come and go

I had in an earlier build had the <a title="A library for promises (CommonJS/Promises/A,B,D)" href="https://www.npmjs.com/package/q" target="_blank">package &#8220;Q&#8221; (promises)</a> as part of the project for one source file. It was built and deployed early in the project&#8217;s life. Later on that source and dependency was removed.

Many builds later the dependency for package &#8220;Q&#8221; was added back into the project. Unfortunately I forgot to add it to bundledDependencies in package.json so it was not included in the zip file used for deployment. Also, the staging environment was rebuilt to change the EC2 instance sizes.

## Why it failed

So here is what happened. Package &#8220;Q&#8221; was already installed from an earlier build on the dev environment but was wiped out on the staging environment because the instances were rebuilt. When the build was deployed to staging the environment did not have &#8220;Q&#8221; package.  The result is the build would not start due to a missing dependency.

## TL;DR

Your project <a title="Specifics of npm's package.json handling" href="https://docs.npmjs.com/files/package.json" target="_blank">package.json</a> may miss a <a title="Array of package names that will be bundled when publishing the package" href="https://docs.npmjs.com/files/package.json#bundleddependencies" target="_blank">bundledDependancies</a> package that may have existed in an earlier build. To ensure you package.json is correct, go to one of the test environment on AWS Elastic Beanstalk and click &#8220;Rebuild Environment&#8221; and retest.
