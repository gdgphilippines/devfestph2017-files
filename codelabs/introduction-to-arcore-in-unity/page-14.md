# 14. Place food on the plane

Now we want to put a tasty bit of food on the plane. This involves creating an object, placing it on a plane, then removing it after some time.

Select the **SceneController object**, and add a new C# script named **FoodController** and begin editing the script.

First, add member variables to reference the plane selected.

1.  The TrackedPlane instance to place the food on.
2.  The instance of the food object.
3.  The age of the object in seconds.
4.  The max age of the food (All food has an expiration date).
5.  An array of prefabs to use to create food instances.

```java
private TrackedPlane trackedPlane;
private GameObject foodInstance;
private float foodAge;
private readonly float maxAge = 10f;

public GameObject[] foodModels;
```

> **Remember:**
> 
> You'll need to add `using GoogleARCore;` to script to resolve the TrackedPlane class!

### Initialize the Food Prefabs

First, make sure to initialize the foodModels array in the editor, by adding models in **Assets/Codelab/Prefab/Foods**. You need to at least add 1, but a varied diet is much more enjoyable!

### Add the food tag

Add a tag in the editor by dropping down the tag selector in the object inspector, and select "Add Tag". Add a tag named "food". We'll use this tag to identify food objects during collision detection.

### Add the SetSelectedPlane method

Create a public method named SetSelectedPlane() that will be called by the SceneController when a plane is selected.

```java
public void SetSelectedPlane(TrackedPlane selectedPlane)
{
    trackedPlane = selectedPlane;
}
```

### Manage the plane state

The plane will change state, size, and position as ARCore interprets the input from the sensors and camera. Since we're holding on to the plane, we need to handle these changes.

In the **FoodController.Update()** method:

*   Check for a null plane; if we don't have a plane we can't do anything.
*   Check for a valid plane. Again, if the plane is invalid, do nothing.

```java
if (trackedPlane == null)
{
    return;
}

if (!trackedPlane.IsValid)
{
    return;
}
```

*   Add the check for the plane being subsumed

```java
// Check for the plane being subsumed
// If the plane has been subsumed switch attachment to the subsuming plane.
while (trackedPlane.SubsumedBy != null)
{
    trackedPlane = trackedPlane.SubsumedBy;
}
```

*   Check if there is no active food instance, and spawn a new one if needed.

```java
if (foodInstance == null || foodInstance.activeSelf == false)
{
    SpawnFoodInstance();
    return;
}
```

*   Lastly, increase the age of existing food and destroy it if expired:

```java
foodAge += Time.deltaTime;
if (foodAge >= maxAge)
{
    DestroyObject(foodInstance);
    foodInstance = null;
}
```

### Implement SpawnFoodInstance()

Spawning a new food item has several steps:

*   Selecting a food prefab.
*   Calculating a random position to spawn the item.
*   Anchoring it to the ARCore frame.
*   Attaching a component to make the item rotate.

```java
private void SpawnFoodInstance ()
{
    GameObject foodItem = foodModels [Random.Range (0, foodModels.Length)];

    // Pick a location.  This is done by selecting a vertex at random and then
    // a random point between it and the center of the plane.
    List<Vector3> vertices = new List<Vector3> ();
    trackedPlane.GetBoundaryPolygon (ref vertices);
    Vector3 pt = vertices [Random.Range (0, vertices.Count)];
    float dist = Random.Range (0.05f, 1f);
    Vector3 position = Vector3.Lerp (pt, trackedPlane.Position, dist);
    // Move the object above the plane.
    position.y += .05f;

    Anchor anchor = Session.CreateAnchor (position, Quaternion.identity);

    foodInstance = Instantiate (foodItem, position, Quaternion.identity,
             anchor.transform);

    // Set the tag.
    foodInstance.tag = "food";

    foodInstance.transform.localScale = new Vector3 (.025f, .025f, .025f);
    foodAge = 0;

    foodInstance.AddComponent<FoodMotion> ();
}
```

### Set the plane from the SceneController

Add the call to `SetSelectedPlane()`in the SceneController.SetSelectedPlane():

```java
// Add to the bottom of SetSelectedPlane() 
GetComponent<FoodController>().SetSelectedPlane(selectedPlane);

```