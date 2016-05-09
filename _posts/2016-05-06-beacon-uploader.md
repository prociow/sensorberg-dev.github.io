---
layout: post
title: "Beacon Uploader"
date: 2016-05-06
comments: true
tags: beacon management platform uploader csv
---

We have been working hard to help you manage beacons effectively. Today we are happy to announce that you can create multiple beacons at once without filling countless browser forms and hitting save buttons endlessly. We offer you a piece of automation.

# Check it out
<a href="https://manage.sensorberg.com/#/beacon/upload">
  <img src="/images/beacon-uploader.png" alt="CSV Beacon Uploader: Beacon Management Platform" style="width: 100%;">
</a>

[https://manage.sensorberg.com/#/beacon/upload](https://manage.sensorberg.com/#/beacon/upload)

# How it works
Uploader takes a CSV file and goes through each line. Concept is simple: one line descrbes one beacon. Our CSV Uploader is smart enough to predict such data as `proximityId`, `major` and `minor`. The only field we require you to fill is `name`, the rest is optional.

# Format we understand
We recommend you using CSV template underneath as a base for creating your own. First of all, note that order of columns matters. This order must remain unchanged. Columns are defined by 1st line of CSV, it is vital.

We recommend you to use `.` as floating number point for `latitude` and `longitude`: e.g. `52.51948`. Please don't use military format coordinates, our system does not understand it.

# CSV template to use
{% highlight csv %}
name,description,proximityId,major,minor,latitude,longitude
Superbeacon,Saved the world,73676723-7400-0000-ffff-0000ffff0001,62169,12870,52.51948,13.39855
{% endhighlight  %}
