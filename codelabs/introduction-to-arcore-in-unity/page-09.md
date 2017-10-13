# 9. Add a floating Scoreboard

In ARCore, objects that maintain a constant position as you move around are positioned by using an [**Anchor**](https://developers.google.com/ar/reference/unity/class/google-a-r-core/anchor). Let's create an Anchor to hold a floating scoreboard.

### Create the Game Object

*   Add an empty game object named Scoreboard.
*   Set the scale of the Scoreboard object to (0.3, 0.3, 0.3). This creates a small scoreboard measuring approximately 1 foot (30 cm).

To the Scoreboard object:

*   Add the **Assets/Codelab/Prefabs/ScoreboardDisplay prefab**.
*   Also add a new script component called ScoreboardController.

### Write the Scoreboard controller script

In order to position the scoreboard, we'll need to know where the user is looking. So we'll add a public variable for the first person camera.

The scoreboard will also be "anchored" to the ARScene. An anchor is an object that holds it position and rotation as ARCore processes the sensor and camera data to build the model of the world.

To keep the anchor consistent with the plane, we'll keep track of the plane and make sure the distance in the Y axis is constant.

Also add a member to keep track of the score.

```java
public Camera firstPersonCamera;
private Anchor anchor;
private TrackedPlane trackedPlane;
private float yOffset;
private int score;
```

> **Remember:**
> 
> You'll need to add `using GoogleARCore;` to script to resolve the Anchor class!

Just as in the previous step, save the script, switch to the scene editor, and set this property to **ARCore Device/First Person Camera** from the scene hierarchy.

We'll place the scoreboard above the selected plane. This way it will be visible and indicate which plane we're focused on.

### Hide until Anchored

We want the scoreboard hidden until it is anchored in position. We'll do this by disabling all the mesh renderers, then once anchored, enable them.

In the Start() method, add the code to disable the mesh renderers.

```java
void Start ()
{
    foreach (Renderer r in GetComponentsInChildren<Renderer>())
    {
        r.enabled = false;
    }
}
```

### Create the function SetSelectedPlane()

This is called from the scene controller when the user taps a plane. When this happens, we'll create the anchor for the scoreboard

```java
public void SetSelectedPlane(TrackedPlane trackedPlane)
{
    this.trackedPlane = trackedPlane;
    CreateAnchor();
}
```

### Create the function CreateAnchor()

The CreateAnchor method does 5 things:

1.  Raycast a screen point through the first person camera to find a position to place the scoreboard.
2.  Create an ARCore Anchor at that position. This anchor will move as ARCore builds a model of the real world in order to keep it in the same location relative to the ARCore device.
3.  Attach the scoreboard prefab to the anchor as a child object so it is displayed correctly.
4.  Record the yOffset from the plane. This will be used to keep the score the same height relative to the plane as the plane position is refined.
5.  Enable the renderers so the scoreboard is drawn.

```java
private void CreateAnchor()
{
    // Create the position of the anchor by raycasting a point towards
    // the top of the screen.
    Vector2 pos = new Vector2 (Screen.width * .5f, Screen.height * .90f);
    Ray ray = firstPersonCamera.ScreenPointToRay (pos);
    Vector3 anchorPosition = ray.GetPoint (5f);

    // Create the anchor at that point.
    if (anchor != null) {
      DestroyObject (anchor);
    }
    anchor = Session.CreateAnchor (anchorPosition, Quaternion.identity);

    // Attach the scoreboard to the anchor.
    transform.position = anchorPosition;
    transform.SetParent (anchor.transform);

    // Record the y offset from the plane.
    yOffset = transform.position.y - trackedPlane.Position.y;

    // Finally, enable the renderers.
    foreach (Renderer r in GetComponentsInChildren<Renderer>())
    {
        r.enabled = true;
    }
}
```

### Add code to ScoreboardController.Update()

First check for tracking to be active.

```java
// The tracking state must be FrameTrackingState.Tracking
// in order to access the Frame.
if (Frame.TrackingState != FrameTrackingState.Tracking)
{
    return;
}
```

Check that there is a selected plane, and update it if it was subsumed by another plane.

```java
// If there is no plane, then return
if (trackedPlane == null)
{
    return;
}

// Check for the plane being subsumed.
// If the plane has been subsumed switch attachment to the subsuming plane.
while (trackedPlane.SubsumedBy != null)
{
    trackedPlane = trackedPlane.SubsumedBy;
}
```

The last thing to add is to rotate the scoreboard towards the user as they move around in the real world and adjust the offset relative to the plane.

```java
// Make the scoreboard face the viewer.
transform.LookAt (firstPersonCamera.transform); 

// Move the position to stay consistent with the plane.
if (trackedPlane.IsUpdated)
{
    transform.position = new Vector3(transform.position.x,
            trackedPlane.Position.y + yOffset, transform.position.z);
} 
```

## Call SetSelectedPlane() from the scene controller

Switch back to the SceneController script and add a member variable for the ScoreboardController.

```java
public ScoreboardController scoreboard; 
```

Save the script, switch back to the scene editor and set this property to the scoreboard object.

Back in SceneController, find the SetSelectedPlane() method we added eariler, and pass the selected plane to the ScoreboardController.

```java
// Add to the end of SetSelectedPlane.
scoreboard.SetSelectedPlane(selectedPlane);
```