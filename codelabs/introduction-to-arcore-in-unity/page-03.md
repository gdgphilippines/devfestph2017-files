# 3. Set up the SceneController

The scene controller is used to coordinate between ARCore and Unity. Create an empty game object and change the name to **SceneController**. Add a C# script component to the object also named SceneController.

### Add ARCore operational checks

Open the script. We need to check for a variety of error conditions. These conditions are also checked in the HelloARExample in the SDK.

First add the _using_ statement to resolve the class name from the ARCore SDK. This will make auto-complete recognize the classes and methods used.

```java
using GoogleARCore;
```

Create a new method named **QuitOnConnectionErrors()** and add it to the bottom of our SceneController. This method checks the state of the ARCore [Session](https://developers.google.com/ar/reference/unity/class/google-a-r-core/session) to make sure ARCore is working in our app.

1.  Is this a supported device? ARCore currently supports a subset of devices running Android Nougat. This list can be found on the developer web site: [https://developers.google.com/ar/discover/#supported_devices](https://developers.google.com/ar/discover/#supported_devices).
2.  Is the permission to use the camera granted? ARCore uses the camera to sense the real world. The user is prompted to grant this permission the first time the application is run. This check is done by ARCore, so you don't have to write any code to check the permission yourself.
3.  Can the ARCore library connect to the AR Services? ARCore relies on AR Services which run on the device in a separate process. If this is missing or not running correctly, the library may not be able to connect to it successfully. In this case restarting the app usually fixes the issue.

```java
private void QuitOnConnectionErrors()
{
    // Do not update if ARCore is not tracking.
    if (Session.ConnectionState == SessionConnectionState.DeviceNotSupported)
    {
        StartCoroutine(CodelabUtils.ToastAndExit(
                "This device does not support ARCore.", 5));
    }
    else if (Session.ConnectionState == SessionConnectionState.UserRejectedNeededPermission)
    {
        StartCoroutine(CodelabUtils.ToastAndExit(
                "Camera permission is needed to run this application.", 5));
    }
    else if (Session.ConnectionState == SessionConnectionState.ConnectToServiceFailed)
    {
        StartCoroutine(CodelabUtils.ToastAndExit(
                "ARCore encountered a problem connecting.  Please start the app again.", 5));
    }
}
```

Now add a call to **QuitOnConnectionErrors()** in the **Start()** method.

```java
  void Start ()
  {
    QuitOnConnectionErrors ();
  }
```

### Check the ARCore tracking state

ARCore needs to capture and process enough information to start tracking the user's movements in the real world. Once ARCore is tracking, the Frame object is used to interact with ARCore. Add this check to the **Update()** method. At the same time, adjust the screen timeout so it stays on if we are tracking.

```java
void Update() 
{
    // The tracking state must be FrameTrackingState.Tracking in order to access the Frame.
    if (Frame.TrackingState != FrameTrackingState.Tracking)
    {
        const int LOST_TRACKING_SLEEP_TIMEOUT = 15;
        Screen.sleepTimeout = LOST_TRACKING_SLEEP_TIMEOUT;
        return;
    }
    Screen.sleepTimeout = SleepTimeout.NeverSleep;
}
```

Great! Now we have the minimum amount of code to start using ARCore and make sure it works. Next, let's try it out!