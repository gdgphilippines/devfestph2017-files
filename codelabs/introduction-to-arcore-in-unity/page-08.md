# 8. Add selecting a plane by tapping

In the SceneController script, add a member variable for the first person camera.

We'll be using the first person camera in this method, so add a public variable and set it to the first person camera.

```java
public Camera firstPersonCamera;
```

Save the script, switch to the scene editor, and set this property to **ARCore Device/First Person Camera** from the scene hierarchy.

To process the touches, we get a single touch and raycast it using the ARCore session to check if the user tapped on a plane. If so, we'll use that one to display the rest of the objects.

Create a new method named ProcessTouches(). This method will perform the ray casting hit test and select the plane that is tapped.

```java
private void ProcessTouches ()
{
    Touch touch;
    if (Input.touchCount != 1 || (touch = Input.GetTouch(0)).phase != TouchPhase.Began)
    {
        return;
    } 

    TrackableHit hit;
    TrackableHitFlag raycastFilter = TrackableHitFlag.PlaneWithinBounds | TrackableHitFlag.PlaneWithinPolygon;

    if (Session.Raycast (firstPersonCamera.ScreenPointToRay (touch.position), raycastFilter, out hit))
    {
      SetSelectedPlane (hit.Plane);
    }
}

```

Create the new method SetSelectedPlane(). This is used to notify all the other controllers that a new plane has been selected. Right now it just logs we selected a plane.

```java
private void SetSelectedPlane(TrackedPlane selectedPlane)
{
    Debug.Log("Selected plane centered at " + selectedPlane.Position);
}
```

The last step is to call ProcessTouches() from Update. Add this code to the end of the Update() method:

```java
// Add to the end of Update()
ProcessTouches();
```

> **Remember:**
>
> Set the `firstPersonCamera` to the first person camera object in the scene editor!