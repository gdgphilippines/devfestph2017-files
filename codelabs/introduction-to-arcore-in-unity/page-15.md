# 15. Eat food and grow!

Now we are moving, add the test for colliding with food, eat it and grow the snake.

Create a new C# script named **FoodConsumer**

We don't want to pollute our awesome snake head prefab with the FoodConsumer, so let's add

It to the instance when we spawn.

In **SnakeController.SpawnSnake()**, add the component to the new instance.

```java
 // After instantiating a new snake instance, add the FoodConsumer component.
 snakeInstance.AddComponent<FoodConsumer>();
```

In FoodConsumer(), add the OnCollisionEnter method (you can delete the boilerplate Start() and Update() methods).

```java
  void OnCollisionEnter(Collision collision) {
    if (collision.gameObject.tag == "food") {
      collision.gameObject.SetActive(false);
      Slithering s = GetComponentInParent<Slithering>();

      if (s != null) {
        s.AddBodyPart();
      } 
    }
  }
```