---
layout: page
title: iOS SDK Integration
permalink: /ios/
additionalNavigation : [
    { "title" : "iOS SDK",                  "link" : "https://github.com/sensorberg-dev/ios-sdk" },
    { "title" : "iOS Bugtracker",           "link" : "https://github.com/sensorberg-dev/ios-sdk/issues" },
    { "title" : "Edit this page",           "link" : "https://github.com/sensorberg-dev/sensorberg-dev.github.io/edit/master/ios.md" }
]
---

# Getting started with the Sensorberg SDK

*This is a guide to help developers get up to speed with Sensorberg iOS SDK. These step-by-step instructions are written for Xcode 7, using the iOS 8 SDK. If you are using a previous version of Xcode, you may want to update before starting.*

## Demo app

Clone the Repository from our [GitHub](https://github.com/sensorberg-dev/ios-sdk).  
Or runing `pod try SensorbergSDK` in a terminal will open the Sensorberg demo project.  
Select the `SBDemoApp` target and run on device.

## Quickstart

### 1. Create an account

To get started with the Sensorberg SDK, [sign up for a free account](https://manage.sensorberg.com/#/signup)
*Read more about our [Beacon Management Platform](https://sensorberg.zendesk.com)*

### 2. Cocoapods

The easiest way to integrate the iOS SDK is via [CocoaPods](https://cocoapods.org/).
*If you're new to CocoaPods, visit their [getting started documentation](https://guides.cocoapods.org/using/getting-started.html).*

````
$ cd your-project-directory
$ pod init
$ open -a Xcode Podfile
````

Once you've initialized CocoaPods, just add the [Sensorberg Pod](https://cocoapods.org/pods/SensorbergSDK) pod to your target:

````
target 'YourApp' do
	pod "SensorbergSDK", "~> {{ site.latestiOSRelease }}"
end
````

Now you can install the dependencies in your project:

````
$ pod install
$ open <YourProjectName>.xcworkspace
````

### 3. Using the SDK

Import the SensorbergSDK

```
# import <SensorbergSDK/SensorbergSDK.h>
```

Setup the SBManager with an **API key** and a **delegate**
*You can find your API key on the [Beacon Managerment Platform](https://manage.sensorberg.com) in the "Apps" section.
The Sensorberg SDK uses an [event bus](https://en.wikipedia.org/wiki/Publish%E2%80%93subscribe_pattern) for events dispatching.
During setup, you pass the class instance that will receive the events as the `delegate`.*

```
[[SBManager sharedManager] setApiKey:<api> delegate:self];
```

Before starting the scanner, we need to ask permission to use the Location services.
If you want to receive events while the app is innactive, you need to pass `YES` to the `requestLocationAuthorization`. If you pass `NO`, the app will receive events only when active.

```
[SBManager sharedManager] requestLocationAuthorization:YES];
```
**Important**: Be sure to add the `NSLocationAlwaysUsageDescription` (or `NSLocationWhenInUseUsageDescription`) key to your plist file and the corresponding string to explain to the user why the app requires access to location.

To start the scanner, simply call `[[SBManager sharedManager] startMonitoring]`.

Then simply **SUBSCRIBE** to the events you want to receive, like *SBEventPerformAction*, *SBeventRegionEnter* etc

```
SUBSCRIBE(SBEventPerformAction) {
	// here you can access the event object which will contain the action to be performed
}
```
Check out the [documentation](http://cocoadocs.org/docsets/SensorbergSDK/) for a list of all supported protocols.
To receive events in other class instances besides the **delegate** call the `REGISTER()` macro and `SUBSCRIBE` to events.


## Documentation
Documentation is available on [CocoaDocs](http://cocoadocs.org/docsets/SensorbergSDK).


## Dependencies

The **Sensorberg SDK 2.1.3 requires iOS 8.0** and uses:

- [JSONModel](https://github.com/icanzilb/JSONModel) version 1.1 for JSON parsing
- [UICKeyChainStore](https://github.com/kishikawakatsumi/UICKeyChainStore) version 2.0 for keychain access
- [tolo](https://github.com/genzeb/tolo) version 1.0.1 for event communication
- [objc-geohash](https://github.com/lyokato/objc-geohash) version 0.0.2 for geocoding system

## Support

If you need any technical support, please file an issue at our [GitHub repository](https://github.com/sensorberg-dev/ios-sdk/issues/new) or contact [our support](https://sensorberg.zendesk.com/hc/en-us/requests/new).

## Commercial Support

Let us know how we can support you with developing awesome applications that include iBeacon functionality. Just [drop us](mailto:support@sensorberg.com) a message.

## Bugs

If you encounter any bugs, please [report them](https://github.com/sensorberg-dev/ios-sdk/issues).

<!--<div class="callout callout-info">-->
<!--    <h1><i class='fa fa-info-circle'></i>Tip: Edit the default beacon regions</h1>-->
<!--    <p>By default, the SDK will monitor all <a href="https://sensorberg.zendesk.com/hc/en-us/articles/201635021-How-is-a-Beacon-ID-structured-">the Sensorberg beacon</a> regions and all the regions you specify at manage.sensorberg.com. If you want to only use the actual regions of your active beacons set the default regions to an empty array:<br> -->
<!--    <pre><code class="language-text" data-lang="text">SBSDKManager.setDefaultRegions(@[])</code></pre>-->
<!--    Please note, you need the <a href="http://sensorberg-dev.github.io/ios-sdk/1.0.2/">1.0.2</a> release to use this feature</p>    -->
<!--</div>-->

<div class="callout callout-info">
    <h1><i class='fa fa-info-circle'></i>Dependency collosions?</h1>
    <p>If you have a dependency collision or you don´t want to integrate the sources of our SDK into your project, <a href="mailto:support@sensorbergcom">contact us</a>. We have <a href="https://github.com/sensorberg-dev/ios-sdk/tree/v2m">a packaged version of the SDK</a> which might be the solution to your problem. <strong>We only recommend to use it in very rare cases!</strong></p>
</div>
