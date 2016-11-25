---
layout: post
title: "Sensorberg iOS SDK v2.3.1"
date: 2016-11-23
comments: true
tags: beacon SDK iOS SensorbergSDK
---
  
### Monitoring Beacon Regions in iOS

Beacon monitoring uses the Bluetooth radio on your iOS device to detect nearby beacons. You need to create a `CLBeaconRegion` and tell the `CLLocationManager` to "monitor" for that region.  
You can create a `CLBeaconRegion` via 3 different initializers:  

1. `initWithProximityUUID:identifier:`  
This allows your app to monitor for *all* the beacons which have a specific proximity UUID. This seems like the best option and it's what most iBeacon SDKs and apps do.  

2. `initWithProximityUUID:major:identifier:`  
Very similar to the previous initializer, this allows you to monitor for all iBeacons with a specific Proximity UUID and a major.

3. `initWithProximityUUID:major:minor:identifier:`  
This creates a CLBeaconRegion for a specific beacon, by 



More precisely, monitoring for beacon regions[^1] will no longer trigger exit events (and subsequently enter events) will not trigger consistently.
With version 2.3.1 of our SDK we changed the behaviour of our SDK to handle this and, if you campaign monitors for 20 beacons or less, we monitor for those specific beacons.  
Monitoring for a specific beacon is more accurate and also helps 


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
