---
layout: post
title: "Secret Codes are now optional"
date: 2015-06-24 17:20:00 +1
comments: true
tags: sdk
---
#Secret Codes are now optional

If you want to use the secret codes feature of the SDK, add this to your manifest. You may also change the actual secret number code by changing the value in `android:host`. See below:


{% highlight java %}
<receiver android:name=".SensorbergCodeReceiver"
    android:process=":sensorberg"
    android:label="sensorberg-logger">
    <intent-filter>
        <action android:name="android.provider.Telephony.SECRET_CODE" />
        <data android:scheme="android_secret_code" android:host="73676723741" />
    </intent-filter>
    <intent-filter>
        <action android:name="android.provider.Telephony.SECRET_CODE" />
        <data android:scheme="android_secret_code" android:host="73676723740" />
    </intent-filter>
</receiver>
{% endhighlight %}

This change comes to affect in the final Android 1.0.1 release.
