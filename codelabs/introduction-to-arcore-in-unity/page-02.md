# 2. Create new Unity project

Create a new Unity 3D project and change the target platform to Android (under File > Build Settings).

![Unity Image](https://codelabs.developers.google.com/codelabs/arcore-intro/img/57d6e085728f079d.png)

Select Android and press Switch Platform.

Then press Player Settings... to configure the Android specific player settings.

### Change player settings for ARCore

*   In Other Settings, disable **Multithreaded rendering**
*   Set the **Package name** to a unique name e.g. _com.<yourname>.arcodelab_
*   Set the **Minimum API** level to 7.0 (Nougat) API level 24

  ![AR Settings Image 1](https://codelabs.developers.google.com/codelabs/arcore-intro/img/6238b2e5c93ab2b8.png)

*   In XR Settings section at the bottom of the list, enable **ARCore Supported**

  ![AR Settings Image 2](https://codelabs.developers.google.com/codelabs/arcore-intro/img/3f638b17ab131d44.png)

### Add the codelab assets

Import **arcore-intro.unitypackage** into your project. (If you haven't already done so, check the Overview step for a list of prerequisites you need to download). This contains prefabs and scripts that will expedite the parts of the codelab so you can focus on how to use ARCore.

### Add the ARCore SDK

Import **arcore-unity-sdk-preview.unitypackage** into your project. (If you haven't already done so, check the Overview step for a list of prerequisites you need to download).

### Add the required scene elements

*   Add **Assets/GoogleARCore/Prefabs/ARCore Device** to your scene.
*   Delete the default Main Camera. We'll use the First Person camera from the ARCore Device prefab.
*   Delete the Directional light.
*   Add the Environmental Light prefab from **Assets/GoogleARCore/Prefabs/Evironmental Light** (Yes, it is spelled wrong - you're living on the bleeding edge of AR development!)
*   Add the event system (from the menu: **GameObject>UI>EventSystem**).

![ARCode Device Image](https://codelabs.developers.google.com/codelabs/arcore-intro/img/3edffa0533ee123a.png)

Now you have a scene setup for using ARCore. Next, let's add some code!