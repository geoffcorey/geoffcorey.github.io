---
{"dg-publish":true,"permalink":"/src/site/notes/src/site/notes/src/site/notes/setup-amazon-aws-wildcard-ssl-certificate/","tags":["aws","ssl","https"]}
---




Wildcard certificates allow you to use unlimited sub-domains with HTTPS/SSL.   Typically if you have 5 or more sub-domains it is cheaper to buy the wildcard certificate then individual subdomain SSL certificates.

Setting up <a title="Amazon Web Services" href="http://aws.amazon.com/" target="_blank">Amazon AWS</a> wildcard certificate requires purchasing a wildcard certificate.   The resulting files are converted to PEM format using <a title="OpenSSL" href="https://www.openssl.org/" target="_blank">openssl</a> command line tool.   To upload the certificates to Amazon AWS you need to using the AWS command line tool.   Once the certificates are on AWS you can use them for <a title="Amazon Elastic Beanstalk" href="http://aws.amazon.com/elasticbeanstalk/" target="_blank">Elastic Beanstalk</a>, <a title="Amazon Elastic Load Balancing" href="http://aws.amazon.com/elasticloadbalancing/" target="_blank">Elastic Load Balancers</a> and serving S3 assets through CloudFront using your own alternate subdomain.

## Purchase a wildcard SSL certificate

Make sure you have an admin email user for the domain (ex. admin-dot-geoffcorey.com)

  * Order certificate from <a title="Wildcard SSL Certificate from Comodo for unlimited sub-domains" href="https://ssl.comodo.com/wildcard-ssl-certificates.php?key5sk0=1907&key5sk1=991e59169d76f8b61023d31b58045940b097e8b1" target="_blank">COMODO</a>
  * Validate you own the domain by checking email for a validation code

### Create a private key

```shell
$ openssl req -key geoffcorey.com.key -out geoffcorey.com.csr -new
```

### Create a CSR


```shell
   $ openssl req -key geoffcorey.com.key -out geoffcorey.com.csr -new
    Enter pass phrase for geoffcorey.com.key:
    You are about to be asked to enter information that will be incorporated
    into your certificate request.
    What you are about to enter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', the field will be left blank.
    -----
    Country Name (2 letter code) [AU]:US
    State or Province Name (full name) [Some-State]:North Carolina
    Locality Name (eg, city) []:Fuquay Varina
    Organization Name (eg, company) [Internet Widgits Pty Ltd]:Geoff Corey
    Organizational Unit Name (eg, section) []:
    Common Name (e.g. server FQDN or YOUR name) []:*.geoffcorey.com
    Email Address []: you know the drill here

    Please enter the following 'extra' attributes
    to be sent with your certificate request
    A challenge password []:
    An optional company name []:
```


To complete your purchase of the wildcard, copy the contents of geoffcorey.com.csr to Comodo order and you will get emailed a zip file containing the following files:

  * STAR\_geoffcorey\_com.ca-bundle
  * STAR\_geoffcorey\_com.pem
  * STAR\_geoffcorey\_com.crt

### Convert to PEM format

Now we need the public and private key in PEM format.

```shell
$ openssl rsa -in geoffcorey.com.key -outform PEM >private.pem
$ openssl x509 -inform PEM -in STAR_geoffcorey_com.crt >public.pem
```

## Setup Amazon AWS wildcard SSL certificate

Now that we have a wildcard certificate we need to add it to our Amazon AWS account and make 2 versions.   The first version is what we will use for Amazon Elastic Beanstalk and Elastic Load Balancers.    The second version is uploaded slightly different and use for Amazon CloudFront to serve S3 assets using our domain with SSL.

### Install AWS CLI

There are various ways to install <a title="Installing the AWS Command Line Tool" href="http://docs.aws.amazon.com/cli/latest/userguide/installing.html" target="_blank">Amazon Command Line tool</a>.   I am using linux with python pip.

```shell
$ sudo pip install --upgrade awscli
```

Create IAM credentials on AWS and get your access key and secret access key.  <a title="Configuring the AWS Command Line tool" href="http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html" target="_blank">Configure awscli</a> tool to use your IAM credentials via

```shell
$ aws configure
```

### Upload certificates

First copy is for server use such as Elastic Beanstalk or Elastic Load Balancers

```shell
$ aws iam upload-servercertificate --server-certificate-name geoffcorey.com --certificate-body file://STAR_geoffcorey_com.pem --private-key file://geoffcorey_com.pem --certificate-chain STAR_geoffcorey_com.ca-bundle
```


This copy is used if you want to have an alternate domain name with Amazon Cloudfront to serve your S3 assest under your domain name.  Note the use of the **&#8211;path** option.

```shell
$ aws iam upload-servercertificate --server-certificate-name geoffcorey.com-cloudfront --certificate-body file://STAR_geoffcorey_com.pem --private-key file://geoffcorey_com.pem --certificate-chain STAR_geoffcorey_com.ca-bundle --path /cloudfront/
```


### Setup Amazon Elastic Beanstalk wildcard SSL certificate

Go to your Elastic Beanstalk app and click Configuration->Load Balancing.

Set your **Secure Listener Port** as 443 and **Protocol** HTTPS and **SSL Certificate ID** to geoffcorey.com.

### Setup Amazon CloudFront wildcard SSL certificate

Go to AWS Cloudfront Manager and click your CloudFront distribution and edit General.

Set your **Alternate Domain Name** to the sub-domain you setup in Route 53.  In my case I will say media.geoffcorey.com then for the **SSL Certificate** select geoffcorey.com-cloudfront.    It is very important that you select **Only Clients that Support Server Name Indication (SNI)** or Amazon will charge you an additional $600/mo.

Details can be found in the <a title="Amazon CloudFront: User Alternate Domain Names (CNAMES)" href="http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/CNAMEs.html#alternate-domain-names-wildcard" target="_blank">developer documentation</a>.
