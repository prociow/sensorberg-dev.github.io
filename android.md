---
layout: page
title: Android SDK Integration
permalink: /android/
additionalNavigation : [
    { "title" : "Source Code",              "link" : "https://github.com/sensorberg-dev/android-sdk" },
    { "title" : "Android SDK Samples",      "link" : "https://github.com/sensorberg-dev/android-sdk-samples" },
    { "title" : "Android SDK Bugtracker",   "link" : "https://github.com/sensorberg-dev/android-sdk/issues" },
    { "title" : "JCenter files",            "link" : "http://jcenter.bintray.com/com/sensorberg/sdk/android-sdk/" },   
    { "title" : "Edit this page",           "link" : "https://github.com/sensorberg-dev/sensorberg-dev.github.io/edit/master/android.md" }
]
---

#How to install the Sensorberg Android SDK 

<div class="callout callout-info">
    <h1><i class='fa fa-info-circle'/></i>Gradle only</h1>
    <p>Gradle is the only supported build system by Google and we will also only support gradle based integrations. Integrating with maven should be identical, but we can provide no support.</p>
</div>

You need to have the jcenter artifactory in your list of repositories and declare the dependency to our <a href="http://jcenter.bintray.com/com/sensorberg/sdk/android-sdk-bootstrapper/{{site.latestAndroidBootstrapperRelease}}/">bootstrapper</a> and the <a href="http://jcenter.bintray.com/com/sensorberg/sdk/android-sdk/{{site.latestAndroidSDKRelease}}/">sdk</a> library.

{% highlight groovy %}
repositories {
    jcenter()
    maven {                 //add this just to be sure, if jcenter is not up to date yet
        url "https://dl.bintray.com/sensorberg/maven/"
    }
}

dependencies {
       compile 'com.sensorberg.sdk:android-sdk-bootstrapper:{{ site.latestAndroidBootstrapperRelease }}'
       compile 'com.sensorberg.sdk:android-sdk:{{ site.latestAndroidSDKRelease }}'
}
{% endhighlight %}

Set your API key in your manifest and declare your <em>BroadcastReceiver</em>:

{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">
    <application>
        <meta-data
            android:name="com.sensorberg.sdk.ApiKey"
            android:value="a8ab23e7f2c4fbdd07d0e0e14835db037d2f62584b976aa0026a671c60e0707f" />
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
<em>You cannot add a Broadcastreceiver at runtime! We´e using a <a href="http://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html">LocalBroadcastManager</a> to send the broadcast and find the receiver(s)</em>

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
        boot.activateService();
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

This class receives a broadcast, if the SDK has detected a beacon and successfully resolved an associated Action. We´re using the Support Library, be aware that this will require an additional dependency:

{% highlight groovy %}
compile "com.android.support:support-v4:22.0.0"
{% endhighlight %}

If you want your own UI to react to detected beacons and actions, please extend the *Bootstrapper* and add your customisations:

{% highlight java %}
public class MyBootstrapper extends SensorbergApplicationBootstrapper {
  public void presentBeaconEvent(BeaconEvent beaconEvent) {
      //your custom code
    }
}
{% endhighlight %}
<div class="callout callout-alert">
    <h1><i class='fa fa-exclamation-triangle'/></i>Process</h1>
    <p>The Bootstrapper runs in your own process, so you are free to access any singletons or statics that you use in your Application. Push the BeaconEvent to your EventBus, Otto and react as you wish.</p>
</div>

<span id="tips"/>
###Development Tips
<div class="callout callout-info">
    <h1><i class='fa fa-info-circle'/></i>Tip: HTTP Debugging</h1>
    <p>You can debug the HTTP communication by enabling the VolleyLog:</p>
    {% highlight bash %}
    adb -shell setprop log.tag.SensorbergVolley VERBOSE
    {% endhighlight %}
</div>

<div class="callout callout-info">
    <h1><i class='fa fa-info-circle'/></i>Tip: Pretty ADB log with Android Bluetooth messages hidden</h1>
    <p>Use <a href="https://github.com/JakeWharton/pidcat">pidcat</a> with grep to show your log and hide the System Bluetooth scan logs:</p>
    {% highlight bash %}
    pidcat com.myapp.packageIdentifier | grep --invert-match BluetoothLeScanner
    {% endhighlight %}
</div>

<div class="callout callout-info">
    <h1><i class='fa fa-info-circle'/></i>Tip: Create Your own account when developing</h1>
    <p>As as developer, you can create an account for free at <a href="https://manage.sensorberg.com/#/signup">manage.sensorberg.com/#/signup</a></p>    
</div>
<div class="callout callout-info">
    <h1><i class='fa fa-info-circle'/></i>Tip: Use the secred codes broadcastreceiver to add more debugging to your app.</h1> 
    <p>Read all about it in this <a href="/2015/06/Tip-howto-remove-secred-codes-receiver/">blog post</a>.</p>    
</div>
<br/>
<br/>
<br/>
<br/>
