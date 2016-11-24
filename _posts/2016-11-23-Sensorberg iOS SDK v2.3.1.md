---
layout: post
title: "Sensorberg iOS SDK v2.3.1"
date: 2016-11-23
comments: true
tags: beacon SDK iOS SensorbergSDK
---
  
### We released [version 2.3.1 of the Sensorberg iOS SDK](https://github.com/sensorberg-dev/ios-sdk)  

With the launch of iOS 10, Apple made some **important** changes to the way CLLocation monitors for beacon regions[^1].  
The biggest change prevents ranging beacons in background - which was possible in previous iOS


# Improvements  

- Added support for iOS 10
- Added support for individual beacons monitoring
- Added support target attributes

# Fixed Bugs  

- Fixed geohash missing from some analytics
- Fixed warnings related to deprecated methods usage

# Related Posts  

- [Conversion measurement[Beta WIP].](http://sensorberg-dev.github.io/2016/06/New-conversion-feature-in-iOS-SDK/)  
- Remote configurations for Campaign  
	- [Actionable Notification with Campaign Payload](http://sensorberg-dev.github.io/2016/06/iOS-Actionable-Notification-with-Payload/)  
	- [Custom Proximity UUIDs for Campaign Simulation](http://sensorberg-dev.github.io/2016/06/Custom-Resolver-URL-API-Key-and-Proximity-UUIDs/)  

Greetings from Berlin!

[^1]: A beacon region is defined only by a UUID, without providing a major or minor for a specific beacon