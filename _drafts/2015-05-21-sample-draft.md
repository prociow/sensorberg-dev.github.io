---
layout: post
title: "this is a sample blog post"
date: 2015-05-21 12:00:00 +1
comments: true
tags: android java open-source
---

#android.permission.READ_SYNC_SETTINGS now optional

If you want your application to 

{% highlight java %}
<uses-permission android:name="android.permission.READ_SYNC_SETTINGS"/>
{% endhighlight %}

This change comes to affect in the final Android 1.0.1 release.

If you want your application with the Sensorberg Android SDK to respect the global users sync settings, just add the line above to your manifest. We will not update the beacon layout and precache data in the background when the user tries to save battery. This settings defaults to true.
