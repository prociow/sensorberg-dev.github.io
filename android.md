---
layout: page
title: Android SDK Integration
permalink: /android/
additionalNavigation : [
    { "title" : "Source Code",              "link" : "https://github.com/sensorberg-dev/android-sdk" },
    { "title" : "Android SDK Samples",      "link" : "https://github.com/sensorberg-dev/android-sdk-samples" },
    { "title" : "Android SDK Bootstrapper", "link" : "https://github.com/sensorberg-dev/android-sdk-bootstrapper" },
    { "title" : "Android SDK Bugtracker",   "link" : "https://github.com/sensorberg-dev/android-sdk/issues" },
    { "title" : "JCenter files",            "link" : "http://jcenter.bintray.com/com/sensorberg/sdk/android-sdk/" },   
    { "title" : "Edit this page",           "link" : "https://github.com/sensorberg-dev/sensorberg-dev.github.io/edit/master/android.md" }
]
---

#How to install the Sensorberg Android SDK 

<div class="callout callout-info">
    <h1><i class='fa fa-info-circle'/> Gradle only</i></h1>
    <p>Gradle is the generally accepted standard for building Android projects, therefore we will only support gradle based integrations. That being said maven technically could be utilised, but we can not provide support</p>
</div>

You need to have the jcenter artifactory in your list of repositories and declare the dependency to our <a href="http://jcenter.bintray.com/com/sensorberg/sdk/android-sdk-bootstrapper/{{site.latestAndroidBootstrapperRelease}}/">bootstrapper</a> and the <a href="http://jcenter.bintray.com/com/sensorberg/sdk/android-sdk/{{site.latestAndroidSDKRelease}}/">sdk</a> library.

{% highlight groovy %}
repositories {
    jcenter()
    maven {//add this just to be sure, if jcenter is not up to date yet.
        url "https://dl.bintray.com/sensorberg/maven/"
    }
}

dependencies {
       compile 'com.sensorberg.sdk:android-sdk-bootstrapper:{{ site.latestAndroidBootstrapperRelease }}'
       compile 'com.sensorberg.sdk:android-sdk:{{ site.latestAndroidSDKRelease }}'
}
{% endhighlight %}

Declare your <em>BroadcastReceiver</em>:

{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">
    <application>
        <receiver android:name="com.myCompany.MyActionPresenter"
            android:process=".sensorberg"
            android:exported:"false" >
            <intent-filter>
                <action android:name="com.sensorberg.android.PRESENT_ACTION" />
            </intent-filter>
        </receiver>
    </application>
</manifest>
{% endhighlight %}
<em>You cannot add a BroadCastReceiver at runtime! We are using a <a href="http://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html">LocalBroadcastManager</a> to send the broadcast and find the receiver(s).</em>

<div class="callout callout-alert">
    <h1><i class='fa fa-exclamation-triangle'/></i>The BroadcastReceiver is running in another process</h1>
    <p>You should be aware, that the Sensorberg Android SDK is running in a separate process. The broadcast will be sent in the separate process as well. The intention of the <em>BroadcastReceiver</em> is to present the content of your <em>Action</em> when the app is in background.</p>
</div>

Enable the SDK in your [Application](http://developer.android.com/reference/android/app/Application.html) object and register foreground/background notifications:

{% highlight java %}
public class DemoApplication extends Application {
    private SensorbergApplicationBootstrapper boot;
    private BackgroundDetector detector;

    @Override
	public void onCreate() {
		super.onCreate();

        boot = new SensorbergApplicationBootstrapper(this);
        //boot = new SensorbergApplicationBootstrapper(this, true); 	use this call if you want presentBeaconEvent(BeaconEvent beaconEvent) in your Bootstrapper to be called
        boot.activateService("<your-API-key>");
        boot.hostApplicationInForeground();

        detector = new BackgroundDetector(boot);
        registerActivityLifecycleCallbacks(detector);
	}
}
{% endhighlight %}

It´s now time to implement the <em>BroadcastReceiver</em>:

{% highlight java %}
public class MyActionPresenter extends BroadcastReceiver {
       @Override
       public void onReceive(Context context, Intent intent) {
           Action action = intent.getExtras().getParcelable(Action.INTENT_KEY);
           switch (action.getType()){
               case MESSAGE_URI:
                   UriMessageAction uriMessageAction = (UriMessageAction) action;
                   showNotification(context, action.getUuid().hashCode(), uriMessageAction.getTitle(), uriMessageAction.getContent(), Uri.parse(uriMessageAction.getUri()));
                   break;
               case MESSAGE_WEBSITE:
                   VisitWebsiteAction visitWebsiteAction = (VisitWebsiteAction) action;
                   showNotification(context, action.getUuid().hashCode(), visitWebsiteAction.getSubject(), visitWebsiteAction.getBody(), visitWebsiteAction.getUri());
                   break;
               case MESSAGE_IN_APP:
                   InAppAction inAppAction = (InAppAction) action;
                   showNotification(context, action.getUuid().hashCode(), inAppAction.getSubject(), inAppAction.getBody(), inAppAction.getUri());
                   break;
           }     
       }   
       
       private void showNotification(Context context, int id, String title, String content, Uri uri) {
           Intent sendIntent = new Intent();
           sendIntent.setAction(Intent.ACTION_SEND);
           sendIntent.putExtra(Intent.EXTRA_TEXT, title + "\n" + content + "\n" + uri.toString());
           sendIntent.setType("text/plain");
   
           Notification notification = new NotificationCompat.Builder(context)
                   .setContentIntent(PendingIntent.getActivity(
                           context,
                           0,
                           new Intent(Intent.ACTION_VIEW, uri),
                           PendingIntent.FLAG_UPDATE_CURRENT))
                   .setContentTitle(title)
                   .setContentText(content)
                   .setSmallIcon(R.drawable.ic_launcher)
                   .setAutoCancel(true)
                   .setShowWhen(true)
                   .build();
           NotificationManager notificationManager = (NotificationManager) context.getSystemService(Context.NOTIFICATION_SERVICE);
           notificationManager.notify(id, notification);
       }
{% endhighlight %}

This class receives a broadcast, if the SDK has detected a beacon and successfully resolved an associated Action. 

If you want your own UI to react to detected beacons and actions, please extend the *Bootstrapper*, make sure you initialize with [*enablePresentationDelegation*](https://github.com/sensorberg-dev/android-sdk-bootstrapper/blob/master/android-sdk-bootstrapper/src/main/java/com/sensorberg/sdk/bootstrapper/SensorbergApplicationBootstrapper.java#L46) by setting to true and adding your customisations:

{% highlight java %}
public class MyBootstrapper extends SensorbergApplicationBootstrapper {
  public void presentBeaconEvent(BeaconEvent beaconEvent) {
      //your custom code
    }
}
{% endhighlight %}

<div class="callout callout-alert">
    <h1><i class='fa fa-exclamation-triangle'/> Process</i></h1>
    <p>The Bootstrapper will run in your own process, so you are free to access any singletons or statics that you use in your Application. Push the BeaconEvent to your EventBus, Otto and react as you wish.</p>
</div>

<span id="tips"/>
###Development Tips
<div class="callout callout-info">
    <h1><i class='fa fa-info-circle'/> Tip: HTTP Debugging</i></h1>
    <p>You can debug the HTTP communication by enabling the VolleyLog:</p>
    {% highlight bash %}
    adb -shell setprop log.tag.SensorbergVolley VERBOSE
    {% endhighlight %}
</div>

<div class="callout callout-info">
    <h1><i class='fa fa-info-circle'/> Tip: Pretty ADB log with Android Bluetooth messages hidden</i></h1>
    <p>Use <a href="https://github.com/JakeWharton/pidcat">pidcat</a> with grep to show your log and hide the System Bluetooth scan logs:</p>
    {% highlight bash %}
    pidcat com.myapp.packageIdentifier | grep --invert-match BluetoothLeScanner
    {% endhighlight %}
</div>

<div class="callout callout-info">
    <h1><i class='fa fa-info-circle'/> Tip: Create Your own account when developing</i></h1>
    <p>As as developer, you can create an account for free at <a href="https://manage.sensorberg.com/#/signup">manage.sensorberg.com/#/signup</a></p>    
</div>
<div class="callout callout-info">
    <h1><i class='fa fa-info-circle'/> Tip: Use the secret codes broadcastreceiver to add more debugging to your app.</i></h1> 
    <p>Read all about it in this <a href="/2015/06/Tip-howto-remove-secred-codes-receiver/">blog post</a>.</p>    
</div>
<br/>
<br/>
