---
layout: post
title: Generating temporary URLs for Bluemix Object Storage
date: 2016-10-21T10:42:00+00:00
author: Geoff Corey
categories:
  - Bluemix
  - Object Storage
  - Swift
tags:
  - bluemix
  - object storage
  - tempurl
  - swift
---
How to use Swift client to generate temporary URL for GET and PUT to Bluemix 
Object Storage.

# Requirements
* Bluemix account
* Add Object Storage Service
* Swift client

## Setup Swift Client

Swift requires some environment variables set to use.  You can get the values
by looking at the Environment section of your provisioned Object Storage
Service VCAP_SERVICES variable for userid, password, and project ID.

{% highlight shell %}
###
# Swift client
###
export OS_USER_ID="<userid>"
export OS_PASSWORD="<password>"
export OS_PROJECT_ID="<project id>"
export OS_AUTH_URL=https://identity.open.softlayer.com/v3
export OS_REGION_NAME=dallas
export OS_IDENTITY_API_VERSION=3
export OS_AUTH_VERSION=3
{% endhighlight %}

## Create a Container

{% highlight shell %}
swift post mycontainer
{% endhighlight %}

## Upload via Temporary URL

Instead of using `swift upload mycontainer myfile` to upload you can create a
temporary URL to give someone to upload without giving out the user ID and
password.   It is temporary access to upload (PUT) or download (GET).

First we need to set a Temp-URL-Key for the account:

{% highlight shell %}
swift post -m Temp-URL-Key:mysecret
{% endhighlight %}

Here I created a script to demonstrate how to use a generate PUT tempurl.

### Save as tmpput.sh
{% highlight shell %}
#!/bin/bash -x
KEY=${1}
CONTAINER=${2}
FILENAME=${3}
ACCOUNT=`swift stat | grep " Account:" | awk '{print $2}'`
echo ${KEY} ${CONTAINER} ${FILENAME} ${ACCOUNT}
export TMPURL=`swift tempurl PUT 3600 /v1/${ACCOUNT}/${CONTAINER}/${FILENAME} ${KEY}`
curl -vvvv -X PUT --upload-file ${FILENAME} https://dal.objectstorage.open.softlayer.com${TMPURL}
{% endhighlight %}

{% highlight shell %}
chmod +x tmpput.sh
./tmpput.sh mysecret mycontainer tmpurl.sh
{% endhighlight %}


## Download via Temporary URL

Same as upload we generate a temporary URL for the GET with an expiration time
of 3600.

### Save snippet as tmpget.sh
{% highlight shell %}
#!/bin/bash -x
KEY=${1}
CONTAINER=${2}
FILENAME=${3}
ACCOUNT=`swift stat | grep " Account:" | awk '{print $2}'`
echo ${KEY} ${CONTAINER} ${FILENAME} ${ACCOUNT}
TMPURL=`swift tempurl GET 3600 /v1/${ACCOUNT}/${CONTAINER}/${FILENAME} ${KEY}`
curl -vvvv https://dal.objectstorage.open.softlayer.com${TMPURL}
{% endhighlight %}

{% highlight shell %}
chmod +x tmpget.sh
./tmpget.sh mysecret mycontainer tmpurl.sh
{% endhighlight %}

You should see the contents from mycontainer/tmpurl.sh displayed on the screen.
