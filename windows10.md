---
layout: page
title: Windows10 SDK Integration
permalink: /windows10/
additionalNavigation : [
{ "title" : "Win10 SDK",                "link" : "https://github.com/sensorberg-dev/windows10-sdk" },
{ "title" : "Win10 Bugtracker",         "link" : "https://github.com/sensorberg-dev/windows10-sdk/issues" },
{ "title" : "Edit this page",           "link" : "https://github.com/sensorberg-dev/sensorberg-dev.github.io/edit/master/windows10.md" }
]
---

# Sensorberg SDK for Windows BETA #

# Please note there is a BETA release. There is some issues that still need to be resolved.
Check the list of [issues](https://github.com/sensorberg-dev/windows10-sdk/issues) to see all issues.

## Compatibility ##

Sensorberg SDK for Windows is supported on Windows 10.

Sensorberg SDK has a dependency to SQLite library, which does not support
"Any CPU" configuration. Thus, your Sensorberg application will only support
x86, x64 and ARM builds.


## Taking Sensorberg SDK into use ##

### Prerequisites ###

VSIX package "Universal App Platform development using Visual Studio 2015 CTP"
needs to be installed to Visual Studio 2015. Package is available at
http://www.sqlite.org/download.html


### 1. Add Sensorberg SDK projects into your solution ###

Right click the solution in **Solution Explorer**, select **Add** and
**Existing Project...**

<img src="/images/site/AddingExistingProject.png" style="width:100%" alt="Adding Sensorberg SDK projects"> 


Browse to the folder where you have the two Sensorberg SDK projects
(`SensorbergSDK` and `SensorbergSDKBackground`) and select the `csproj` files of
both projects. Note that you may have to add the projects one by one.

### 2. Add Sensorberg SDK project references ###

Add the two SDK projects to your application project as references. Right click
**References** under your application project and select **Add Reference...**

<img src="/images/site/AddingReference.png" style="width:50%" alt="Adding reference"> 

Locate the two SDK projects and make sure that the check boxes in front of them
are checked and click **OK**.
 
<img src="/images/site/AddingSDKProjectsAsReference.png" style="width:100%" alt="Adding SDK projects as references"> 


### 3. Declare background tasks in manifest file ###

Add the following `Extensions` into your `Package.appxmanifest` file:

```xml
  <Applications>
    <Application Id="App"
      Executable="$targetnametoken$.exe"
      EntryPoint="MySensorbergApp.App">

      ...

      <Extensions>
        <Extension Category="windows.backgroundTasks" EntryPoint="SensorbergSDKBackground.AdvertisementWatcherBackgroundTask">
          <BackgroundTasks>
            <Task Type="bluetooth" />
          </BackgroundTasks>
        </Extension>
        <Extension Category="windows.backgroundTasks" EntryPoint="SensorbergSDKBackground.TimedBackgroundTask">
          <BackgroundTasks>
            <Task Type="timer" />
          </BackgroundTasks>
        </Extension>
      </Extensions>
      
      ...
      
    </Application>
  </Applications>
```


### 4. Declare capabilities in manifest file ###

Make sure that you have at least `internetClient` and `bluetooth` capabilities
declared in your `Package.appxmanifest` file:

```xml
  ...
  
  </Applications>

  <Capabilities>
    <Capability Name="internetClient" />
    <DeviceCapability Name="bluetooth" />
  </Capabilities>
</Package>
```


### 5. Take SDKManager into use ###

The following snippet demonstrates how to integrate the Sensorberg SDK
functionality to the main page of your application:

```C#
using SensorbergSDK;
using System;
using Windows.UI.Popups;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Navigation;

namespace MySensorbergApp
{
    public sealed partial class MainPage : Page
    {
        private SDKManager _sdkManager;
        
        public MainPage()
        {
            this.InitializeComponent();

            _sdkManager = SDKManager.Instance(0x1234, 0xBEAC); // Manufacturer ID and beacon code
            
            _sdkManager.BeaconActionResolved += OnBeaconActionResolvedAsync;
            Window.Current.VisibilityChanged += SDKManager.Instance.OnApplicationVisibilityChanged;
        }

        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            base.OnNavigatedTo(e);

            BeaconAction pendingBeaconAction = BeaconAction.FromNavigationEventArgs(e);

            if (pendingBeaconAction != null)
            {
                _sdkManager.ClearPendingActions();                

                if (await pendingBeaconAction.LaunchWebBrowserAsync())
                {
                    Application.Current.Exit();
                }
                else
                {
                    OnBeaconActionResolvedAsync(this, pendingBeaconAction);
                }
            }

            _sdkManager.InitializeAsync("04a709a208c83e2bc0ec66871c46d35af49efde5151032b3e865768bbf878db8");
            await _sdkManager.RegisterBackgroundTaskAsync();
        }
        
        private async void OnBeaconActionResolvedAsync(object sender, BeaconAction e)
        {
            await Windows.ApplicationModel.Core.CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(
                Windows.UI.Core.CoreDispatcherPriority.Normal, async () =>
                {
                    MessageDialog messageDialog = e.ToMessageDialog();
                    await messageDialog.ShowAsync();
                });
        }
    }
}        
```

In `OnNavigatedTo` method we first check for pending actions. If the background
task has created a notification and the user clicks/taps the notification, your
application is launched with the pending action in `NavigationEventArgs`. It is
recommended to check the pending actions before initializing the SDK. Otherwise,
**all** the pending actions are delivered to `OnBeaconActionResolvedAsync` event
handler. Note that we clear all the pending actions and react only to the action
associated with the notification the user clicked/tapped. All the notifications
"remember" their actions so calling `ClearPendingActions` method will not affect
them.

Sensorberg SDK is initialized with `InitializeAsync` method, which takes your
API key as the only argument. For creating your service and API key, visit
https://manage.sensorberg.com

You must implement the handling of the beacon actions in
`OnBeaconActionResolvedAsync()`. In the example above, we simply display a
message dialog with the action content.

It is also highly recommended to ask the user for the permission to enable the
background task. Notifications are created automatically by the background task.
You can register and unregister the background task using `SDKManager` methods
`RegisterBackgroundTaskAsync` and `UnregisterBackgroundTask`.
