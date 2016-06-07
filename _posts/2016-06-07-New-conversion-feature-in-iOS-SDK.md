---
layout: post
title: "New conversion feature in iOS SDK"
date: 2016-06-07
comments: true
tags: beacon iOS tracking conversion SDK
---

#Track Your User Actions with New Conversion Feature.

The "Conversion" feature enables you to track user actions with beacons and campaigns. with this feature you can get the information how many users were around the beacon region, how many campaign were delivered to the app and how many campaigns were performed by users.

<!--more-->

**New "action" property in  SBMCampaignAction**

This "action" property is unique id to report conversion information.

{% highlight objective-c %}

@interface SBMCampaignAction : NSObject
...
// action : unique action fire event identifier
@property (strong, nonatomic) NSString      *action; 
@end

{% endhighlight %}

When you receive "SBEventPerformAction" event from SDK, 'SBMCampaignAction' object is set in the event (if it doesn't have error :D).
Now you can use new 'action' parameter to report conversion through 'SBManager'

**Currently we have following 4 conversion types.**

```

/**
 SBConversionType
 Represents the conversion type for a specific campaign action
 @since 2.1.2
 */
typedef enum : NSUInteger {
    /**
     *  The campaign action can't "fire" (ex. the user has denied access to local notifications)
     */
    kSBConversionUnavailable = -2,
    /**
     *  The campaign was suppressed by the app
     */
    kSBConversionSuppressed = -1,
    /**
     *  The campaign action has been "fired" but was ignored by the user
     *
     * @discussion: The campaigns are marked as "ignored" by default. To correctly measure conversion, be sure to call [SBManager sharedManager] reportConversion: forCampaignAction:] when performing the campaign action
     */
    kSBConversionIgnored = 0,
    /**
     *  The campaign action has been performed successfully
     */
    kSBConversionSuccessful = 1
} SBConversionType;

```

**Example for kSBConversionUnavailable : when you cannot notify action to current user**

{% highlight objective-c %}

SUBSCRIBE(SBEventPerformAction)
{
	if (![[SBManager sharedManager] canReceiveNotifications])
    {
        [[SBManager sharedManager] reportConversion:kSBConversionUnavailable forCampaignAction:[action.action copy]];
    }
    else
    {
	    // schedule notification
   }
}

{% endhighlight }

**Example for kSBConversionSuccessful :  When user did react with notification**

{% highlight objective-c %}

- (void)application:(UIApplication *)application didReceiveLocalNotification:(UILocalNotification *)notification
{
    if (notification.userInfo) {
        NSDictionary *dict = [notification.userInfo valueForKey:@"action"];
        SBMCampaignAction *action = [SBUtilities campaignActionFromDictionary:dict];
        if (action)
        {
	        [[SBManager sharedManager] reportConversion:kSBConversionSuccessful forCampaignAction:[action.action copy]];
    }
   }
}

{% endhighlight }

In this case you can also show alert and let your customer decide to do action or not.

**Default Conversion value for campaign action is "kSBConversionIgnored". **
**We overwrite conversion value when you report conversion through 'SBManager' **

Enjoy the Beacon !!

Sensorberg Tech


