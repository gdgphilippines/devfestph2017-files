# 12. Like and dislike functionality

We now have our `updateDatabase()` function prepared, and the only thing we need to do now is to call this method with the appropriate function parameter in the `dislike()` and `like()` methods.

Recall that the updateDatabase() function returns a Future, which means we need to use await and mark the dislike() method as async.

▶️ Update `dislike()` to look like this:
```javascript
dislike() async {
  count = await updateDatabase((c) => c - 1);
}
```
What's happening here? We call `updateDatabase()` with `(c) => c - 1` as the only parameter. That's a function that takes a value and decrements it by one. We than _await until_ `updateDatabase` returns the latest value of the counter. Remember, we're working with a live, real-time database, so the value could have changed in unexpected ways.

> The arrow (`=>`) is a shorthand for Dart's function body with a single `return` statement. It's equivalent to `(c) { return c - 1; }`.

▶️  Update `like()` to look like this:
```javascript
like() async {
  count = await updateDatabase((c) => c + 1);
}
```

> If you see an error underlining the `await` keyword in `like()` or `dislike()`, make sure the methods are marked as `async` (see above).

▶️  Now refresh the app in the web browser.

You should now be able to change the value in the counter. You can open multiple browser windows with the app and see how the value changes in all windows to the same value. This is because the Firebase database is real-time.

![](https://codelabs.developers.google.com/codelabs/angulardart-firebase-web-app/img/a375b95dcf7e5f7e.png)

## [**`SEE THE SOURCE CODE`**](https://github.com/Janamou/firebase-counter-steps/tree/master/5-likedislike/firebase_counter)

We could stop here and say that our app is done. But let's make it even nicer and use some material design components instead of our buttons!





