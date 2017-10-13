# 5. Scale the objects to reality

Unity's scaling system is designed so that when working with the Physics engine, 1 unit of distance can be thought of as 1 meter in the real world. ARCore is designed with this assumption in mind. We use this scaling system to scale virtual objects so they look reasonable in the real world.

For example, an object placed on a desktop should be small enough to be on the desktop. A reasonable starting point would be ½ foot (15.24 cm), so the scale should (0.1524, 0.1524, 0.1524).

This might not look the best in your application, but it tends to be a good starting point and then you can fine tune the scale further for your specific scene.

As a convenience, the prefabs used in this codelab contain a component named GlobalScalable which supports using stretch and pinch to size the objects. To enable this, the touch input needs to be captured.

### Add GlobalScalable support to the Scene Controller

Select the scene controller object in the hierarchy and add the Script component Global Scalable. In the properties for the component, enable "Handle Scale Input" and disable "Adjust Scale." The scale of the scene controller should not be adjusted since it is the parent of the ARCore detected planes.

![ARCore Image](https://codelabs.developers.google.com/codelabs/arcore-intro/img/aed2c8070a6e51c8.png)

Now when running the application, the user can pinch or stretch the objects to fit the scene more appropriately.

Next, let's detect and display the planes that are detected by ARCore.