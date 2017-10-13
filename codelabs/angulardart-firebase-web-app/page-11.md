# 11. Prepare to use transactions

Now we'll implement the like and dislike functionality. The Firebase database is realtime, but because we can do lots of very fast and concurrent modifications, we need to use the `transaction()` method, which atomically updates data at the actual database location.

The `transaction()` method takes a function as a parameter. This function should update an existing value into the new value. Firebase ensures that there are no conflicts with other clients writing to the same location at the same time. If there would be a conflict, Firebase updates the data from the database and runs your function again using the updated data.

Both the `dislike()` and `like()` methods need to use this transaction, so let's wrap this functionality into a new method called `updateDatabase()`.

The `updateDatabase()` method takes an update function (a closure) as a parameter. This function is used for data manipulation in the transaction. The `updateDatabase()` function then returns a [Future](https://api.dartlang.org/stable/1.23.0/dart-async/Future-class.html) object. (A `Future` represents a means for getting a value asynchronously.)

▶️  Import `dart:async` in your `app_component.dart` to be able to use `Future`:
```css
import 'dart:async';
```

▶️  Add the following code to the AppComponent class:

```javascript
Future<int> updateDatabase(Function update) async {
  var transaction = await ref.transaction((current) {
    if (current != null) {
      current = update(current);
    }
    return current;
  });

  return transaction.snapshot.val();
}
```

You call the `transaction()` function on your [DatabaseReference](https://www.dartdocs.org/documentation/firebase/3.1.0/firebase/DatabaseReference-class.html) property. It asynchronously updates the `current` value using the `update` function and returns the new value of `current`.

> **How does this async magic work?** The `transaction()` function is asynchronous: it returns a `Future` object with a "future" value of a [Transaction](https://www.dartdocs.org/documentation/firebase/3.1.0/firebase/Transaction-class.html). We want to _wait_ for that value. You can do this in within a function that's marked as `async` (such as `updateDatabase()`) by putting the `await` keyword before the asynchronous function call (such as the call to `transaction()`). The return type of the `async` function (`updateDatabase()`) must be `Future`.

Once we have the `Transaction` value, we want to get the current value in the counter from the snapshot and return it, so we can show it in our counter. To do so, our method has to return a `Future` object. Because the retrieved value from the snapshot is `int`, our method returns a `Future<int>`.

### Optional: Typedef

This small step is **completely optional** and is intended for better code quality. Feel free to skip it and continue to the next step.

The `updateDatabase()` method takes a `Function`—any function—as a parameter. But it'd be better to restrict what arguments the function can have, and what it can return. Dart's `typedef` can help with this.

▶️  Create a definition (`typedef`) for the function outside the `AppComponent` class (typedefs cannot be parts of a class). The function should take only one parameter (of type `T`) and return an object of the same type (`T`). Call the typedef `UpdateFunction`:
```javascript
typedef T UpdateFunction<T>(T value);
```

▶ Now use the typedef in the `updateDatabase()` method:
```javascript
Future<int> updateDatabase(UpdateFunction<int> update) async {
  var transaction = await ref.transaction((current) {
    if (current != null) {
      current = update(current);
    }
    return current;
  });

  return transaction.snapshot.val();
}
```


