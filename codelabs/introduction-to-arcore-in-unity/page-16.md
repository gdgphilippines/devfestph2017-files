# 16. Use the scoreboard

Remember that Scoreboard from the beginning of the codelab? Well, now it is time to actually use it!

In **SceneController.Update()**, set the score to the length of the snake:

```java
 scoreboard.SetScore(snakeController.GetLength());
```

Now we need to implement SetScore in **ScoreboardController**:

```java
public void SetScore(int score)
{
    if (this.score != score)
    {
        GetComponentInChildren<TextMesh>().text = "Score: " + score;
        this.score = score;
    }
}
```

Add GetLength() in the **SnakeController**

```java
public int GetLength()
{
    return GetComponent<Slithering>().GetLength();
}
```