---
layout: post
title: "Secret Codes are now optional"
date: 2015-06-24 17:20:00 +1
comments: true
tags: sdk
---
#Tip: Remove Secret Codes Broadcastreceiver from your manifest

If you don´t want to ship the secret codes feature of the SDK, add this to your *application* part of your manifest.

{% highlight xml %}
<receiver
   android:name="com.sensorberg.sdk.SensorbergCodeReceiver"
   tools:node="remove"
   tools:selector="com.sensorberg.sdk" />
{% endhighlight %}

<!--more-->

<div class="callout callout-info">
    <h1><i class='fa fa-info-circle'/></i>Check your manifest with apktool</h1>
    <p>To make sure this worked, test your final APK with <a href="http://ibotpeaches.github.io/Apktool/">apktool</a>:</p>
{% highlight bash %}
apktool d -s -f <your-apk-file>
{% endhighlight %}
</div>

