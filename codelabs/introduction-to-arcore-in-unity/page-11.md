# 11. Snake on the plane

Now that we have a plane, let's put a snake on it and move it around on the plane.

Create a new Empty Game object named **Snake**.

Add the existing C# script (**Assets/Codelab/Scripts/Slithering.cs**). This controls the movement of the snake as it grows. In the interest of time, we'll just add it, but feel free to review the code later on.

Add a new C# script to the Snake named **SnakeController**.

We need to track the plane that the snake is traveling on. We'll also add member variables for the prefab for the head, and the instance:

```java
private TrackedPlane trackedPlane;

public GameObject snakeHeadPrefab;
private GameObject snakeInstance;
```

> **Remember:**
> 
> You'll need to add `using GoogleARCore;` to script to resolve the TrackedPlane class!

## Set the prefabs

Back in the editor, set the prefab values. For the snakeHeadPrefab, use **Assets/Codelab/Prefabs/SnakeHeadPrefab**. For the snakeBody in the Slithering component, use **Assets/Codelab/Prefabs/SnakeBodyPrefab**.

## Create the SetPlane() method

Add a method to set the plane. When the plane is set, spawn a new snake.

```java
public void SetPlane (TrackedPlane plane)
{
    trackedPlane = plane;
    // Spawn a new snake.
    SpawnSnake();
}
```

Then spawn the snake. Set the initial position slightly above the plane, so the snake drops onto the plane.

```java
private void SpawnSnake ()
{
    if (snakeInstance != null)
    {
        DestroyImmediate (snakeInstance);
    }

    Vector3 pos = trackedPlane.Position;
    pos.y += 0.1f;

    // Not anchored, it is rigidbody that is influenced by the physics engine.
    snakeInstance = Instantiate (snakeHeadPrefab, pos,
            Quaternion.identity, transform);

    // Pass the head to the slithering component to make movement work.
    GetComponent<Slithering> ().Head = snakeInstance.transform;
}
```

Now add a member variable to the **SceneController** to reference the Snake.

```java
public SnakeController snakeController;
```

Save the script, switch to the scene editor and assign the Snake object to snakeController in the scene inspector.

In **SceneController.SetSelectedPlane()**, pass the selected plane to the snake controller

```java
// Add to SetSelectedPlane()
snakeController.SetPlane(selectedPlane);
```