# 1. Overview

ARCore is a platform for building augmented reality apps on Android. ARCore uses three key technologies to integrate virtual content with the real world as seen through your phone's camera:

*   [**Motion tracking**](https://developers.google.com/ar/discover/concepts#motion_tracking) allows the phone to understand and track its position relative to the world.
*   [**Environmental understanding**](https://developers.google.com/ar/discover/concepts#environmental_understanding) allows the phone to detect the size and location of flat horizontal surfaces like the ground or a coffee table.
*   [**Light estimation**](https://developers.google.com/ar/discover/concepts#light_estimation) allows the phone to estimate the environment's current lighting conditions.

This codelab guides you through building a simple demo game to introduce these capabilities so you can use them in your own applications.

### What you'll learn

*   Enabling ARCore through the Player settings
*   Adding the ARCore SDK prefabs to the scene
*   Scaling objects consistently to look reasonable with respect to the real world.
*   Using Anchors to place objects at a fixed location relative to the real world.
*   Using detected planes as the foundation of augmented reality objects
*   Using touch and gaze input to interact with the ARCore scene

### Prerequisites

Make sure you have these before starting the codelab:

*   Unity 2017.2 beta 11 ([https://unity3d.com/unity/beta](https://unity3d.com/unity/beta/archive))
*   [ARCore compatible device](https://developers.google.com/ar/discover/#supported_devices) and cable
*   [ARCore.apk](https://github.com/google-ar/arcore-android-sdk/releases/download/sdk-preview/arcore-preview.apk) (from our developer site)
*   [ARCore.unitypackage](https://github.com/google-ar/arcore-unity-sdk/releases/download/sdk-preview/arcore-unity-sdk-preview.unitypackage) (from our developer site)
*   [Sample assets for the project](https://github.com/googlecodelabs/arcore-intro/raw/master/arcore-intro.unitypackage) (hosted on GitHub)

More information about getting started can be found at: [https://developers.google.com/ar/develop/unity/getting-started](https://developers.google.com/ar/develop/unity/getting-started)

Now that you have everything you need, let's start!