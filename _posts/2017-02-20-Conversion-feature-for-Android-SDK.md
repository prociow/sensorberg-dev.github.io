---
layout: post
title: "Conversion feature for Android SDK added in 2.2.0"
date: 2017-02-20
comments: true
tags: beacon Android tracking conversion SDK
---

# Tracking User Actions with the Sensorberg Conversion Feature.

The "Conversion" feature enables you to measure user interactions with beacons and campaigns.
With this feature you can track the following informations:

- How many users were around the beacon region
- How many campaigns were delivered to the app
- How many campaign actions were performed by users

<!--more-->

**Currently we have following 3 conversion types**
{% highlight java %}
    /**
     * Action was given to the host app, but host app did not return confirmation
     * that it made attempt to show it to the user. This is the situation where e.g.
     * app delays showing notification to the user for whatever reason.
     */
    public static final int TYPE_SUPPRESSED = -1;

    /**
     * App has confirmed via SensorbergSdk#notifyActionShowAttempt(UUID, Context)
     * that the action was shown to  the user by notification or otherwise.
     */
    public static final int TYPE_IGNORED = 0;

    /**
     * App has confirmed via SensorbergSdk#notifyActionSuccess(UUID, Context)
     * that the user acknowledged the action (e.g. user opened notification).
     */
    public static final int TYPE_SUCCESS = 1;
{% endhighlight %}

**Conversion lifecycle**

- Conversion begins its life as "**Suppressed**" when our Android SDK generates the Action,
even before it's passed to the host app. This requires no work on your host application side.

- Next stage is marking the Action as "**Ignored**", which means that you've attempted to show it to the user,
but you have no knowledge if the user interacted with your notification / alert.
To do this call:
{% highlight java %}
  SensorbergSdk.notifyActionShowAttempt(sdkAction.getUuid(), context);
{% endhighlight %}

- Last stage is marking the Action as "**Success**" which means that user interacted with your notification.
For this to happen call:
{% highlight java %}
  SensorbergSdk.notifyActionSuccess(sdkAction.getUuid(), context);
{% endhighlight %}

**Please note**:

 - Conversion value for given Action (as identified by UUID) can only move in one direction.
    This means the order of events is **Suppressed -> Ignored -> Success**. (what has been seen cannot be unseen)
    As a result trying to e.g. mark Action that was "Success" as "Ignored" will have no effect.
 - You may skip "Ignored" stage if that is what you want and mark Action as "Success" straight away.

In case you need out-of-the-box solution that works with conversions and Notifications we've got you covered.
Extend the abstract [ActionReceiver](https://github.com/sensorberg-dev/android-sdk/blob/master/android-sdk/src/main/java/com/sensorberg/ActionReceiver.java "ActionReceiver") class and supply your Notification in **onGetNotification**, plus implement
**onAction** (or onVisitWebsiteAction / onInAppAction / onUriAction if you want to be more specific) callbacks,
and the rest of conversion will happen automatically when user taps on it. Remember to put the ActionReceiver extended class in AndroidManifest.xml and use remote process for it.

{% highlight xml %}
<receiver
    android:name=".receivers.MyActionReceiver"
    android:exported="false"
    android:process=".sensorberg">
    <intent-filter>
        <action android:name="com.sensorberg.android.PRESENT_ACTION"/>
        <action android:name="com.sensorberg.android.CONVERSION_SUCCESS"/>
        <action android:name="com.sensorberg.android.CONVERSION_DELETE"/>
    </intent-filter>
</receiver>
{% endhighlight %}

[Corresponding blogpost about conversions for iOS](https://developer.sensorberg.com/2016/06/New-conversion-feature-in-iOS-SDK/ "Corresponding blogpost about conversions for iOS")

Enjoy the Conversions!
