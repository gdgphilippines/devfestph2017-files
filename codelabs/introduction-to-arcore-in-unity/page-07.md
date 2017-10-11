# 7. Add plane detection

ARCore detects horizontal planes. We'll use these planes in the game. For each newly detected plane, we'll create a game object that renders the plane using a prefab.

### Add the prefab member variable

Open the SceneController script and add this member variable:

```java
public GameObject trackedPlanePrefab;
```

Save the script, and switch back to the scene editor. In the editor, use the property Inspector to set the value to the prefab made in the previous step: **Assets/TrackedPlaneDisplay**.

### Create ProcessNewPlanes() method

Continue editing the SceneController script by adding the ProcessNewPlanes method to iterate through the newly detected planes and creates a game object to visualize the plane

```java
private void ProcessNewPlanes()
{
    List<TrackedPlane> planes = new List<TrackedPlane>();
    Frame.GetNewPlanes(ref planes);

    for(int i=0;i<planes.Count;i++)
    {
        // Instantiate a plane visualization prefab and set it to track the new plane.
        // The transform is set to the origin with an identity rotation since the mesh
        // for our prefab is updated in Unity World coordinates.
        GameObject planeObject = Instantiate(trackedPlanePrefab, Vector3.zero,
                Quaternion.identity, transform);
        planeObject.GetComponent<TrackedPlaneController>().SetTrackedPlane(planes[i]);
    }
}
```

In **Update(),** add the call to ProcessNewPlanes():

```java
// Add this to the bottom of Update
ProcessNewPlanes();
```

### Save and Run

Now save the scene & project and run the app. As you look at the room, ARCore will detect planes and they should appear as different color grids. As ARCore detects more about the scene, the plane will change shape and merge with, or _subsume_ other planes.

Depending on the physical characteristics of the environment, it might take a couple seconds for the first plane to be detected. Once a plane is detected, it will be rendered using a random color. Plane detection can be improved with good lighting and some sort of pattern or texture (like wood grain, or a rug design).

![Plane Detection Image](https://codelabs.developers.google.com/codelabs/arcore-intro/img/8211c28935a65cfd.png)