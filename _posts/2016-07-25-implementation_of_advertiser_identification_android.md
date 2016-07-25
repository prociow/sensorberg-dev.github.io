---
layout: post
title: "How do I... implement the Advertiser Identifier in Android?"
date: 2016-07-25
comments: true
tags: beacon sdk android
---
For the Advertiser Identifier there are multiple ways of implementing it depending on your application. We
will show an example of what can be done. Of course the most import thing is to set, and to get the Id, how
you do that is where the implementation can differ.


- Set the identifier. In our dev app you can see in the application class a good example how to set. Please note,
you will most likely want input from the user, like a switch or button.
{% highlight java %}
new Thread(new Runnable() {
               @Override
               public void run() {
                   long timeBefore = System.currentTimeMillis();
                   try {
                       AdvertisingIdClient.Info info = AdvertisingIdClient.getAdvertisingIdInfo(getApplicationContext());
                       if (info == null || info.getId() == null) {
                           Logger.log.logError("AdvertisingIdClient.getAdvertisingIdInfo returned null");
                           return;
                       }
                       boot.setAdvertisingIdentifier(info.getId());
                   } catch (IOException e) {
                       Logger.log.logError("could not fetch the advertising identifier beacuse of an IO Exception", e);
                   } catch (GooglePlayServicesNotAvailableException e) {
                       Logger.log.logError("play services not available", e);
                   } catch (GooglePlayServicesRepairableException e) {
                       Logger.log.logError("play services need repairing", e);
                   } catch (Exception e) {
                       Logger.log.logError("could not fetch the advertising identifier beacuse of an unknown error", e);
                   }
                   Logger.log.verbose("fetching the advertising identifier took " + (System.currentTimeMillis() - timeBefore) + " millis");
               }
           }).start();
       }
{% endhighlight %}

- If you need to unset, you can set the identifier to null. 

{% highlight java %}
ShowcaseApplication.getInstance().boot.setAdvertisingIdentifier(null);
{% endhighlight %}

The user can reset the google identifier through their device settings.

-The Sensorberg Android Team
