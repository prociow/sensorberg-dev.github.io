---
layout: post
title: "Set Custom Resolver URL, API Key and Proximity UUIDs for Beacon Simulation"
date: 2016-06-10
comments: true
tags: beacon showcase iOS
---
#Set API Key and Resolver URL manually, and Simulate Beacon with Custom Proximity UUIDs on iOS Showcase App

iOS Showcase app is able to do following 3 actions.
  
1. Simulate a beacon with custom proximity UUIDs
2. Change the Resolver URL on the App
3. Set the API Key Manually on the App instead of scanning QR-Code
  
<!--more-->
  
### Simulate beacon with custom proximity UUIDs
  
In Showcase app, we have the 'Beacon Simulation' functionality. And it was available to simulate beacon just with predefined proximity UUID in the app.  But now we can add custom proximity UUID and simulate beacon with it.
  
**Add custom proximity UUIDs in 'Advanced option' of your application**
  
1. Select your application.
2. Select the Advanced option.
3. Enter your custom UUIDs with name.
4. Save the iOS Settings.
5. Save Application.
  
Ex : custom UUIDs
  
```javascript
{
  "customProximityUUIDs": {
    "124C5678-4444-1111-2222-134556728422": "SB-GUNNIH"
  }
}
```
  
<iframe width="560" height="315" src="https://www.youtube.com/embed/ZIMk9KxAW4k" frameborder="0" allowfullscreen></iframe>

  
*And now you can see your custom proximity UUID in the app.*

<iframe width="375" height="667" src="https://www.youtube.com/embed/nqwsBmGphQw" frameborder="0" allowfullscreen></iframe>
  
**Change the Resolver URL on the App**
  
1. Open the App.
2. Switch to Status Screen.
3. Tab on 'Reachable' Item in 'Sensorberg Platform' section.
4. Enter your own Resolver URL on the Textfield of Alert.
5. Select 'Ok' button on Alert.
  
<iframe width="375" height="667" src="https://www.youtube.com/embed/AAtOiItxLeg" frameborder="0" allowfullscreen></iframe>
    
**Set the API Key Manually on the App instead of scanning QR-Code**
  
1. Open the App.
2. Switch to Status Screen.
3. Tab on 'Valid API Key' Item in 'Sensorberg Platform' section.
4. Or tab on 'Scan QR Code For API Key' item in 'Application' section. when the scanner comes up, tab 'Enter manually' button.
5. Enter or Paste 'API Key' on the Textfield of Alert.
6. Select 'Ok' button on Alert.
  
<iframe width="375" height="667" src="https://www.youtube.com/embed/Ej6XP8K_WsI" frameborder="0" allowfullscreen></iframe><iframe width="375" height="667" src="https://www.youtube.com/embed/ZyvCgHSvpO0" frameborder="0" allowfullscreen></iframe>