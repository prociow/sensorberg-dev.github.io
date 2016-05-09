---
layout: post
title: "Beacon Uploader"
date: 2016-05-06
comments: true
tags: beacon management platform uploader csv
---

We have been working hard to help you manage beacons effectively. Today we are happy to announce that you can create multiple beacons at once without filling countless browser forms and hitting save buttons endlessly. This process can now be automated using our CSV Beacon Upload Tool.

# Check it out
<a href="https://manage.sensorberg.com/#/beacon/upload">
  <img src="/images/beacon-uploader.png" alt="CSV Beacon Uploader: Beacon Management Platform" style="width: 100%;">
</a>

[https://manage.sensorberg.com/#/beacon/upload](https://manage.sensorberg.com/#/beacon/upload)

# How it works
The Beacon Uploader takes a CSV file, goes through each line, and if all of the required information in the line is valid a new beacon is added to your Sensorberg account based on that information. The concept is pretty simple: one line describes one beacon. Our Beacon Uploader is smart enough to create valid entries for a beacon's `proximityId`, `major` and `minor`. The only field we actually require you to fill in is `name`, the rest is optional.

# Format we understand
We recommend you using CSV template underneath as a base for creating your own. First of all, note that order of columns matters. This order must remain unchanged. The columns must be defined in the first line of your CSV, and they must match this example.

Please use a decimal value for `latitude` and `longitude`: e.g. `52.51948`. Please don't use military format coordinates, our system does not understand it. If you need to convert your military formatted coordinates to decimal format, you may be able to do so using this [GPS Conversion Tool](http://www.findlatitudeandlongitude.com/gps-coordinates-converter/#.VwZwQRN97y8).

# CSV template example
{% highlight csv %}
name,description,proximityId,major,minor,latitude,longitude
Entrance Beacon,South Entrance closest to parking,73676723-7400-0000-ffff-0000ffff0001,62169,12870,52.51946,13.39851
Registration Beacon,Check-in desk where bracelets are handed out,73676723-7400-0000-ffff-0000ffff0001,62169,12873,52.51948,13.39855
{% endhighlight  %}
