---
layout: post
title: "How to configure SOLR for Geo-Spatial Search"
modified:
excerpts: This post explains how to configure SOLR for Geo-Spatial or Location Search
tags: [SOLR, Search]
image:
  feature:
date: 2014-11-15T13:27:50+05:30
---
Last week, We at MySmartPrice worked on public pilot for displaying offline brick & mortar store price of selected mobiles for all Hyderabad users. We rely heavily on caching for our delivery and this feature is going to be real time delivery based on the user location. I ended up trying SOLR for performing the geo-spatial location search for getting the stores close to the user location. More details about the integration will be posted shortly in [MySmartPrice Tech Blog](http://tech.mysmartprice.com).

## Why I migrated to Jekyll
It sounds cool. Powering my blog using Jekyll sounded like fun and cool as it is not so easy for non-tech bloggers. Leaving this useless reason, I thought it will be good to try out some other blog engine which is not wordpress. When I started searching for alternatives, Jekyll came highly recommended. Hosting a static blog removes all the hassle of managing the mysql database and wordpress installation which was haunting me since i migrated to digital ocean for my blog. The MySQL instance goes down due to insufficient memory even though the server serves some 1000 users a day through Wordpress installation. Most probably due to some misconfiguration but who has time to check it out anyway. So i took a big step and migrated my posts to markdown.

## Jekyll Theme and Hosting
I found [Minimal Mistakes](http://mmistakes.github.io/minimal-mistakes/) by Michael Rose. It is a very good theme which was minimalistic and also provides all the basic features I needed in my blog. So I cloned the repo and source controlled it at my [github account](https://github.com/arunchinnachamy/arunchinnachamy.com). Once I have the files in my mac, I followed the steps at [Theme Setup](http://mmistakes.github.io/minimal-mistakes/theme-setup/) which is very handy to start. I started tweaking the theme files and also edited the configuration files to fit my needs. Once all the posts are in place and configuration is complete, I tested the blog in my local machine to make sure the blog is complete and is working.

## Deployment to S3
Now it is time to deploy to S3. As my blog is on top-level domain, I migrated my DNS from GoDaddy to Amazon Route 53. Meanwhile, I created a container in Amazon S3 which will hold all my blog files. Make sure the static hosting is enabled in the container and copy the web hosting URL provided by S3.  Once the name servers are changed in the registrar, create a new domain in Route 53 with alias to the hosting URL provided by S3. Now we are set to deliver the contents from S3. Only thing which is pending is uploading the site contents to the container. I used [s3cmd](http://s3tools.org/s3cmd) and followed the script [here](http://www.savjee.be/2013/02/howto-host-jekyll-blog-on-amazon-s3/) to complete my setup.

Now I am writing this post in my iAWriter and with a execution of single shell script will be live at my blog hosted in Amazon S3. It is neat and elegant. 
