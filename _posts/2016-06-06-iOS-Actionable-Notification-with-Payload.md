---
layout: post
title: "iOS Actionable Notification with Payload"
date: 2016-06-06
comments: true
tags: beacon showcase iOS
---
#Customise Your notification with Payload

Since 2015 April, we have payload feature[[see this](http://sensorberg-dev.github.io/2015/04/Unlimited-Use-Cases-With-Action-Payload)] in [management platform](https://manage.sensorberg.com/#/applications).  
In this post we will highlight the usage of payload for [actionable notification on iOS](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIMutableUserNotificationAction_class/) and show different kind of usages of payload.

##Use case description:
We'd like to show an actionable notification with custom contents from the payload which user edited on [management platform](https://manage.sensorberg.com/#/applications).  With this article you can see how to set actionable notification with custom payload and handle it.  

<!--more-->
**Add content in payload of your campaign**

<iframe width="560" height="315" src="https://www.youtube.com/embed/36ZR_Metkrw" frameborder="0" allowfullscreen></iframe>
1. Select a campaign first.
2. Select Advanced option.
3. Write custom payload contents
4. Save the Campaign.

"onEnter" key to use the contents for actionable notification.

- "onEnter" object will contain contents for the action which has triggerType "On Enter"
- "onExit" object will contain contents for the action which has triggerType "On Exit"

"confirmationAction" can have one of following 2 options : *open*, *share*  

- open	: open the URL which is given in campaign.
- share : share the URL, which is given in campaign, on Facebook or other.

"actInForeground" is boolean value

- if it's true, app will be in foreground and do something when user did select 'ok' button or tab directly on the notification. otherwise app will process "confirmationAction" in background.

Following 3 ("de", "en", "ko") objects are localised strings for notification. Of course you can add more languages with [ISO 639-1](https://en.wikipedia.org/wiki/ISO_639-1) language codes.

- "okButtonTitle" : ok button title on notification.
- "cancelButtonTitle" : cancel button title on notification.
- "title": subject on notification
-  "message" : message on notification

Ex : custom payload

```javascript
{
  "onEnter": {
    "confirmAction": "open",
    "actInForeground": true,
    "de": {
      "okButtonTitle": "Zur Website",
      "cancelButtonTitle": "Schließen",
      "title": "Hallo!!",
      "message": "Wilkommen in Sensorberg!"
    },
    "en": {
      "okButtonTitle": "Open Website",
      "cancelButtonTitle": "Close",
      "title": "Hi!!",
      "message": "Welcome to Sensorberg!"
    },
    "ko": {
      "okButtonTitle": "사이트 열기",
      "cancelButtonTitle": "닫기",
      "title": "안녕하세요!!",
      "message": "Sensorberg에 오신것을 환영합니다!"
    }
  }
}
```

**Get content of payload from SBCampaignAction**

To get SBMCampaignAction, you need to subscribe "SBEventPerformAction" with Tolo.

- <code> SUBSCRIBE(SBEventPerformAction){ SBMCampaignAction *action = event.campaign; } </code>

And check payload in SBMCampaigAnction

- <code> if (action.payload) { ... } </code>

Get "onEnter" content from payload

- % highlight objective-c % NSDictionary *actionDict =  [action.payload @"onEnter"] % endhighlight %

Register UserNotificationSettings.

% highlight objective-c %
    
    NSDictionary *localizedDict = [actionDict objectForKey:@"en"];
    UIMutableUserNotificationAction *okAction = [UIMutableUserNotificationAction new];
    okAction.identifier = @"okActionIdentifier";
    okAction.title = [localizedDict objectForKey:@"okButtonTitle"];
    okAction.authenticationRequired = NO;
    okAction.activationMode = [[actionDict objectForKey:@"actInForeground"] boolValue] ?UIUserNotificationActivationModeForeground : UIUserNotificationActivationModeBackground;
    
    UIMutableUserNotificationAction *cancelAction = [UIMutableUserNotificationAction new];
    cancelAction.identifier = @"cancelButtonIdentifer";
    cancelAction.title = [localizedDict objectForKey:@"cancelButtonTitle"];
    cancelAction.authenticationRequired = NO;
    // cancel action should not activate app.
    cancelAction.activationMode = UIUserNotificationActivationModeBackground;
    UIMutableUserNotificationCategory *actionCategory;
    actionCategory = [[UIMutableUserNotificationCategory alloc] init];
    [actionCategory setIdentifier:@"myOnEnterActionCategoryIdeintifier"];
    [actionCategory setActions:@[okAction, cancelAction] forContext:UIUserNotificationActionContextDefault];
    
    NSSet *categories = [NSSet setWithObject:actionCategory];
    UIUserNotificationType types = (UIUserNotificationTypeAlert|
                                    UIUserNotificationTypeSound|
                                    UIUserNotificationTypeBadge);
    
    UIUserNotificationSettings *settings;
    settings = [UIUserNotificationSettings settingsForTypes:types categories:categories];
    
    [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
    
% endhighlight %


Create Local notification with notification category and schedule notification.


% highlight objective-c %

UILocalNotification *n = [UILocalNotification new];
n.alertTitle = [localizedDict objectForKey:@"title"];
n.alertBody = [localizedDict objectForKey:@"message"];
n.category =  @"myOnEnterActionCategoryIdeintifier";
n.userInfo =@{@"action": @"User Info what you need to handle"};
if (action.fireDate)
{
    n.fireDate = action.fireDate;
}
else
{
    n.fireDate = [NSDate dateWithTimeIntervalSinceNow:1];
}
    
[[UIApplication sharedApplication] scheduleLocalNotification:n];

% endhighlight %

#The Result

Let's see how it is working on iPhone :D

<iframe width="375" height="667" src="https://www.youtube.com/embed/2xqAm41u08E" frameborder="0" allowfullscreen></iframe><iframe width="375" height="667" src="https://www.youtube.com/embed/9lDainFUTZs" frameborder="0" allowfullscreen></iframe>

