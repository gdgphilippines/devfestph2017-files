# 12. Add Gaze to direct the snake

To move the snake, we'll use where we are looking as a point that the snake should move towards. To do this, we'll raycast the center of the screen through the ARCore session to a point on a plane.

First add a game object that we'll use to visualize where the user is looking.

Edit the SnakeController and add member variables for the pointer and the first person camera. Also add a speed member variable.

```java
public GameObject pointer;
public Camera firstPersonCamera;
// Speed to move.
public float speed = 20f;
```

### Set the game object properties

Save the script and switch to the scene editor.

Add an instance of the **Assets/CodelabPrefabs/gazePointer** to the scene.

Then, set the pointer property to the instance of the gazePointer, and the firstPersonCamera to the ARCore device's first person camera.

### Update the pointer state

In **SnakeController.Update()** add a check for the snake being active. If it is not, just return; there is nothing to do.

```java
if (snakeInstance == null || snakeInstance.activeSelf == false) {
    pointer.SetActive(false);
    return;
}
else
{
    pointer.SetActive(true);
}
```

### Raycast the center of the screen

If we are active, then create a ray based on the center of the screen:

```java
    Ray ray =  firstPersonCamera.ScreenPointToRay(new Vector2
      (Screen.width/2, Screen.height/2));
```

Use the ARCore Session to raycast and if there is a hit, use that point, but average the point on the plane's y position with the snake head's so the pointer is between the plane and the head.

```java

    TrackableHit hit;
    TrackableHitFlag raycastFilter = TrackableHitFlag.PlaneWithinBounds;    

    if (Session.Raycast (ray, raycastFilter, out hit))
    {
      Vector3 pt = hit.Point;
      //Set the Y to the Y of the snakeInstance
      pt.y = snakeInstance.transform.position.y;
      // Set the y position relative to the plane and attach the pointer to the plane
      Vector3 pos = pointer.transform.position;
      pos.y = pt.y;
      pointer.transform.position = pos; 

      // Now lerp to the position                                         
      pointer.transform.position = Vector3.Lerp (pointer.transform.position, pt,
        Time.smoothDeltaTime * speed);
    }
```

### Move towards the pointer

Once the snake is heading in the right direction, move towards it. We want to stop before the snake is at the same spot to avoid a weird nose spin.

```java
    // Move towards the pointer, slow down if very close.                                                                                     
    float dist = Vector3.Distance (pointer.transform.position,
        snakeInstance.transform.position) - 0.05f;
    if (dist < 0)
    {
      dist = 0;
    }

    Rigidbody rb = snakeInstance.GetComponent<Rigidbody> ();
    rb.transform.LookAt (pointer.transform.position);
    rb.velocity = snakeInstance.transform.localScale.x *
        snakeInstance.transform.forward * dist / .01f;

```