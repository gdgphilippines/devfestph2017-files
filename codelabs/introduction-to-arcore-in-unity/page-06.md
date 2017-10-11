# 6. Create plane display prefab

ARCore uses a class named [TrackedPlane](https://developers.google.com/ar/reference/unity/class/google-a-r-core/tracked-plane) to represent detected planes. This class is not a game object, so we need to make a prefab that will render the tracked planes.

In the scene editor, add a 3D plane object and rename it **TrackedPlaneDisplay**. Make sure its transform is reset to (0,0,0) and scale of (1,1,1).

Expand the Mesh Renderer component and set the material to **PlaneGrid** (it is part of the Example scene in the ARCore SDK).

Since the TrackedPlane returns a position of the center of a polygon and the list of vertices defining that polygon, we need a script to pass that information from TrackedPlane into the Unity mesh renderer. This is pre-written in the interest of time for the codelab. Add the script **Assets/Codelab/Scripts/PlaneRenderer** to the TrackedPlaneDisplay.

### Create the TrackedPlaneController script

Create a new script attached to the TrackedPlaneDisplay named **TrackedPlaneController**. This script will be used to interact with the ARCore API.

Add 3 member variables:

1.  The TrackedPlane object to render.
2.  The reference to the PlaneRenderer component to avoid calling GetComponent() repeatedly.
3.  A list of points that store the polygon vertices.

```java
private TrackedPlane trackedPlane;
private PlaneRenderer planeRenderer;
private List<Vector3> polygonVertices = new List<Vector3> ();
```

> **Remember:**
>
> You'll need to add `using GoogleARCore;` to script to resolve the TrackedPlane class!

Initialize the planeRenderer reference in Awake

```java
void Awake()
{
    planeRenderer = GetComponent<PlaneRenderer>();
}
```

Add the **SetTrackedPlane()** method. This sets the trackedPlane, gets the polygon vertices and initializes the plane renderer.

```java
public void SetTrackedPlane (TrackedPlane plane)
{
    trackedPlane = plane;
    trackedPlane.GetBoundaryPolygon (ref polygonVertices);
    planeRenderer.Initialize ();
    planeRenderer.UpdateMeshWithCurrentTrackedPlane(trackedPlane.Position,
        polygonVertices);
}
```

Add the Update() method. This handles the changes in the tracked plane state.

```java
void Update ()
{
    // If no plane yet, disable the renderer and return.
    if (trackedPlane == null)
    {
        planeRenderer.EnablePlane (false);
        return;
    }

    // If this plane was subsumed by another plane, destroy this object, the other
    // plane's display will render it.
    if (trackedPlane.SubsumedBy != null)
    {
        Destroy (gameObject);
        return;
    }

    // If this plane is not valid or ARCore is not tracking, disable rendering.
    if (!trackedPlane.IsValid || Frame.TrackingState != FrameTrackingState.Tracking)
    {
        planeRenderer.EnablePlane (false);
        return;
    }

    // OK! Valid plane, so enable rendering and update the polygon data if needed.
    planeRenderer.EnablePlane (true);

    if (trackedPlane.IsUpdated)
    {
        trackedPlane.GetBoundaryPolygon (ref polygonVertices);
        planeRenderer.UpdateMeshWithCurrentTrackedPlane (trackedPlane.Position,
                polygonVertices);
    }
}
```

That's it! Save the scripts, and switch back to the scene editor.

### Save as a prefab

The **TrackedPlaneDisplay** object is complete. Drag it from the scene Hierarchy to the Assets folder to save it as a prefab.

Once it is copied, delete it from the scene.