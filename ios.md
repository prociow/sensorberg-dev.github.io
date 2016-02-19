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

#How to install the Sensorberg iOS SDK
## Usage

To run the example project, clone the repo, and open the Xcode project in the SBDemoApp directory.

## Requirements

The Sensorberg SDK requires iOS 8.0.

## Installation

Sensorberg SDK uses [CocoaPods](http://cocoapods.org).

To install it, simply add the following line to your Podfile:

    pod "SensorbergSDK", "~> {{ site.latestiOSRelease }}"
                                                                           

and follow the implementation in the [Demo app](https://github.com/sensorberg-dev/ios-sdk/tree/master/SBDemoApp).

Check the [BeaconsViewController](https://github.com/sensorberg-dev/ios-sdk/blob/master/SBDemoApp/BeaconsViewController.m) ```viewDidLoad``` method for the minimal integration.
Also check the [cocoadocs documentation](http://cocoadocs.org/docsets/SensorbergSDK/{{site.latestiOSRelease}}/) and the included [documentaion](https://github.com/sensorberg-dev/ios-sdk/tree/master/docs)

## Support

If you need any technical support, please file an issue at our [GitHub repository](https://github.com/sensorberg-dev/ios-sdk/issues/new) or contact [our support](https://sensorberg.zendesk.com/hc/en-us/requests/new).

## Commercial Support

Let us know how we can support you with developing awesome applications that include iBeacon functionality. Just [drop us](mailto:support@sensorberg.com) a message.

## Bugs

If you encounter any bugs, please [report them](https://github.com/sensorberg-dev/ios-sdk/issues).

<div class="callout callout-info">
    <h1><i class='fa fa-info-circle'/></i>Tip: Edit the default beacon regions</h1>
    <p>By default, the SDK will monitor all <a href="https://sensorberg.zendesk.com/hc/en-us/articles/201635021-How-is-a-Beacon-ID-structured-">the Sensorberg beacon</a> regions and all the regions you specify at manage.sensorberg.com. If you want to only use the actual regions of your active beacons set the default regions to an empty array:<br> 
    <pre><code class="language-text" data-lang="text">SBSDKManager.setDefaultRegions(@[])</code></pre>
    Please note, you need the <a href="http://sensorberg-dev.github.io/ios-sdk/1.0.2/">1.0.2</a> release to use this feature</p>    
</div>
<br/>
<br/>
<br/>
<br/>
<br/>

