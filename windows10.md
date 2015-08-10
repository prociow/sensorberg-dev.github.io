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

<div class="callout callout-info">
    <h1><i class='fa fa-info-circle'/></i>Please note there is a BETA release. There are some issues that still need to be resolved.</h1>
    Check the list of <a href="https://github.com/sensorberg-dev/windows10-sdk/issues">issues</a> to see all issues.
</div>


## Compatibility ##

Sensorberg SDK for Windows is supported on Windows 10.


Sensorberg SDK has a dependency to SQLite library, which does not support
"Any CPU" configuration. Thus, your Sensorberg application will only support
x86, x64 and ARM builds.


## <a id="user-content-taking-sensorberg-sdk-into-use" class="anchor" href="#taking-sensorberg-sdk-into-use" aria-hidden="true"><span class="octicon octicon-link"></span></a>Taking Sensorberg SDK into use ##

### <a id="user-content-prerequisites" class="anchor" href="#prerequisites" aria-hidden="true"><span class="octicon octicon-link"></span></a>Prerequisites ###

VSIX package "Universal App Platform development using Visual Studio 2015 CTP"
needs to be installed to Visual Studio 2015. Package is available at
<a href="http://www.sqlite.org/download.html">http://www.sqlite.org/download.html</a>

### <a id="user-content-1-add-sensorberg-sdk-projects-into-your-solution" class="anchor" href="#1-add-sensorberg-sdk-projects-into-your-solution" aria-hidden="true"><span class="octicon octicon-link"></span></a>1. Add Sensorberg SDK projects into your solution ###

Right click the solution in __Solution Explorer__, select __Add__ and
__Existing Project...__

<img src="/images/site/AddingExistingProject.png" alt="Adding Sensorberg SDK projects" style="width:100%">

Browse to the folder where you have the two Sensorberg SDK projects
(<code>SensorbergSDK</code> and <code>SensorbergSDKBackground</code>) and select the <code>csproj</code> files of both projects. Note that you may have to add the projects one by one.

### <a id="user-content-2-add-sensorberg-sdk-project-references" class="anchor" href="#2-add-sensorberg-sdk-project-references" aria-hidden="true"><span class="octicon octicon-link"></span></a>2. Add Sensorberg SDK project references ###

Add the two SDK projects to your application project as references. Right click
__References__ under your application project and select __Add Reference...__

<img src="/images/site/AddingReference.png" alt="Adding reference" style="max-width:100%;">

Locate the two SDK projects and make sure that the check boxes in front of them
are checked and click __OK__.

<img src="/images/site/AddingSDKProjectsAsReference.png" alt="Adding SDK projects as references" style="max-width:100%;">

### <a id="user-content-3-declare-background-tasks-in-manifest-file" class="anchor" href="#3-declare-background-tasks-in-manifest-file" aria-hidden="true"><span class="octicon octicon-link"></span></a>3. Declare background tasks in manifest file ###

Add the following <code>Extensions</code> into your <code>Package.appxmanifest</code> file:

<div class="highlight highlight-xml"><pre>  &lt;<span class="pl-ent">Applications</span>&gt;
    &lt;<span class="pl-ent">Application</span> <span class="pl-e">Id</span>=<span class="pl-s"><span class="pl-pds">"</span>App<span class="pl-pds">"</span></span>
      <span class="pl-e">Executable</span>=<span class="pl-s"><span class="pl-pds">"</span>$targetnametoken$.exe<span class="pl-pds">"</span></span>
      <span class="pl-e">EntryPoint</span>=<span class="pl-s"><span class="pl-pds">"</span>MySensorbergApp.App<span class="pl-pds">"</span></span>&gt;

      ...

      &lt;<span class="pl-ent">Extensions</span>&gt;
        &lt;<span class="pl-ent">Extension</span> <span class="pl-e">Category</span>=<span class="pl-s"><span class="pl-pds">"</span>windows.backgroundTasks<span class="pl-pds">"</span></span> <span class="pl-e">EntryPoint</span>=<span class="pl-s"><span class="pl-pds">"</span>SensorbergSDKBackground.AdvertisementWatcherBackgroundTask<span class="pl-pds">"</span></span>&gt;
          &lt;<span class="pl-ent">BackgroundTasks</span>&gt;
            &lt;<span class="pl-ent">Task</span> <span class="pl-e">Type</span>=<span class="pl-s"><span class="pl-pds">"</span>bluetooth<span class="pl-pds">"</span></span> /&gt;
          &lt;/<span class="pl-ent">BackgroundTasks</span>&gt;
        &lt;/<span class="pl-ent">Extension</span>&gt;
        &lt;<span class="pl-ent">Extension</span> <span class="pl-e">Category</span>=<span class="pl-s"><span class="pl-pds">"</span>windows.backgroundTasks<span class="pl-pds">"</span></span> <span class="pl-e">EntryPoint</span>=<span class="pl-s"><span class="pl-pds">"</span>SensorbergSDKBackground.TimedBackgroundTask<span class="pl-pds">"</span></span>&gt;
          &lt;<span class="pl-ent">BackgroundTasks</span>&gt;
            &lt;<span class="pl-ent">Task</span> <span class="pl-e">Type</span>=<span class="pl-s"><span class="pl-pds">"</span>timer<span class="pl-pds">"</span></span> /&gt;
          &lt;/<span class="pl-ent">BackgroundTasks</span>&gt;
        &lt;/<span class="pl-ent">Extension</span>&gt;
      &lt;/<span class="pl-ent">Extensions</span>&gt;

      ...

    &lt;/<span class="pl-ent">Application</span>&gt;
  &lt;/<span class="pl-ent">Applications</span>&gt;</pre></div>

### <a id="user-content-4-declare-capabilities-in-manifest-file" class="anchor" href="#4-declare-capabilities-in-manifest-file" aria-hidden="true"><span class="octicon octicon-link"></span></a>4. Declare capabilities in manifest file ###

Make sure that you have at least <code>internetClient</code> and <code>bluetooth</code> capabilities
declared in your <code>Package.appxmanifest</code> file:

<div class="highlight highlight-xaml"><pre>  ...

  &lt;/<span class="pl-ent">Applications</span>&gt;

  &lt;<span class="pl-ent">Capabilities</span>&gt;
    &lt;<span class="pl-ent">Capability</span> <span class="pl-e">Name</span>=<span class="pl-s"><span class="pl-pds">"</span>internetClient<span class="pl-pds">"</span></span> /&gt;
    &lt;<span class="pl-ent">DeviceCapability</span> <span class="pl-e">Name</span>=<span class="pl-s"><span class="pl-pds">"</span>bluetooth<span class="pl-pds">"</span></span> /&gt;
  &lt;/<span class="pl-ent">Capabilities</span>&gt;
&lt;/<span class="pl-ent">Package</span>&gt;</pre></div>

### <a id="user-content-5-take-sdkmanager-into-use" class="anchor" href="#5-take-sdkmanager-into-use" aria-hidden="true"><span class="octicon octicon-link"></span></a>5. Take SDKManager into use ###

The following snippet demonstrates how to integrate the Sensorberg SDK
functionality to the main page of your application:

<div class="highlight highlight-cs"><pre><span class="pl-k">using</span> SensorbergSDK;
<span class="pl-k">using</span> System;
<span class="pl-k">using</span> Windows.UI.Popups;
<span class="pl-k">using</span> Windows.UI.Xaml;
<span class="pl-k">using</span> Windows.UI.Xaml.Controls;
<span class="pl-k">using</span> Windows.UI.Xaml.Navigation;

<span class="pl-k">namespace</span> <span class="pl-en">MySensorbergApp</span>
{
    <span class="pl-k">public</span> <span class="pl-k">sealed</span> <span class="pl-k">partial</span> <span class="pl-k">class</span> <span class="pl-en">MainPage</span> : <span class="pl-k">Page</span>
    {
        <span class="pl-k">private</span> SDKManager _sdkManager;

        <span class="pl-k">public</span> <span class="pl-en">MainPage</span>()
        {
            <span class="pl-c1">this</span>.InitializeComponent();

            _sdkManager = SDKManager.Instance(<span class="pl-c1">0x1234</span>, <span class="pl-c1">0xBEAC</span>); <span class="pl-c">// Manufacturer ID and beacon code</span>

            _sdkManager.BeaconActionResolved += OnBeaconActionResolvedAsync;
            Window.Current.VisibilityChanged += SDKManager.Instance.OnApplicationVisibilityChanged;
        }

        <span class="pl-k">protected</span> <span class="pl-k">override</span> <span class="pl-k">async</span> <span class="pl-k">void</span> <span class="pl-en">OnNavigatedTo</span>(<span class="pl-k">NavigationEventArgs</span> <span class="pl-smi">e</span>)
        {
            <span class="pl-c1">base</span>.OnNavigatedTo(e);

            BeaconAction pendingBeaconAction = BeaconAction.FromNavigationEventArgs(e);

            <span class="pl-k">if</span> (pendingBeaconAction != <span class="pl-c1">null</span>)
            {
                _sdkManager.ClearPendingActions();

                <span class="pl-k">if</span> (<span class="pl-k">await</span> pendingBeaconAction.LaunchWebBrowserAsync())
                {
                    Application.Current.Exit();
                }
                <span class="pl-k">else</span>
                {
                    OnBeaconActionResolvedAsync(<span class="pl-c1">this</span>, pendingBeaconAction);
                }
            }

            _sdkManager.InitializeAsync(<span class="pl-s"><span class="pl-pds">"</span>04a709a208c83e2bc0ec66871c46d35af49efde5151032b3e865768bbf878db8<span class="pl-pds">"</span></span>);
            <span class="pl-k">await</span> _sdkManager.RegisterBackgroundTaskAsync();
        }

        <span class="pl-k">private</span> <span class="pl-k">async</span> <span class="pl-k">void</span> <span class="pl-en">OnBeaconActionResolvedAsync</span>(<span class="pl-k">object</span> <span class="pl-smi">sender</span>, <span class="pl-k">BeaconAction</span> <span class="pl-smi">e</span>)
        {
            <span class="pl-k">await</span> Windows.ApplicationModel.Core.CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(
                Windows.UI.Core.CoreDispatcherPriority.Normal, <span class="pl-k">async</span> () =&gt;
                {
                    MessageDialog messageDialog = e.ToMessageDialog();
                    <span class="pl-k">await</span> messageDialog.ShowAsync();
                });
        }
    }
}        </pre></div>

In <code>OnNavigatedTo</code> method we first check for pending actions. If the background
task has created a notification and the user clicks/taps the notification, your
application is launched with the pending action in <code>NavigationEventArgs</code>. It is
recommended to check the pending actions before initializing the SDK. Otherwise,
<strong>all</strong> the pending actions are delivered to <code>OnBeaconActionResolvedAsync</code> event
handler. Note that we clear all the pending actions and react only to the action
associated with the notification the user clicked/tapped. All the notifications
"remember" their actions so calling <code>ClearPendingActions</code> method will not affect
them.

Sensorberg SDK is initialized with <code>InitializeAsync</code> method, which takes your
API key as the only argument. For creating your service and API key, visit
<a href="https://manage.sensorberg.com">https://manage.sensorberg.com</a>

You must implement the handling of the beacon actions in
<code>OnBeaconActionResolvedAsync()</code>. In the example above, we simply display a
message dialog with the action content.

It is also highly recommended to ask the user for the permission to enable the
background task. Notifications are created automatically by the background task.
You can register and unregister the background task using <code>SDKManager</code> methods
<code>RegisterBackgroundTaskAsync</code> and <code>UnregisterBackgroundTask</code>.
<p><br>
<br></p>
