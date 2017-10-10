# 9. Load data from Firebase

We'll use our `ngOnInit()` method to load data from the Firebase Realtime Database, a cloud-hosted NoSQL database that stores data as JSON.

Our Firebase database structure looks like this:

![](https://codelabs.developers.google.com/codelabs/angulardart-firebase-web-app/img/ca647bc6e56ee171.png)

We have a `counter` path with some numeric value in the database. Our application will connect to this database and load the current value of counter.

First, we need to get the reference to our database `counter` path. In the library, we have a `DatabaseReference` object for this and we'll store it in a `ref` instance variable in our component's class.

In this step, you **edit lib/app_component.dart**.

▶️ Create the `ref` instance variable:
```javascript
class AppComponent implements OnInit {
  int count = 0;
```

▶️  In `ngOnInit()`, get the database reference to our `'counter'` path from the database method (_after_ the call to `initializeApp()`):
```cs
ngOnInit() {
  initializeApp(
      ...);

  ref = database().ref('counter');
};
```
To load the data using the `'counter`' path, you can listen to the `onValue` stream. [Several event streams](https://www.dartdocs.org/documentation/firebase/3.1.0/firebase/DatabaseReference-class.html) are available, but `onValue` is good when you want to listen to the event on the initial load, and then again each time the data changes.

▶️  Add the following to `ngOnInit()` to start updating the app's `count` property according to the newest database value:

```javascript
ref.onValue.listen((e) {
  count = e.snapshot.val();
});
```


How does this work? When the `value` event appears, the handler function specified to `onValue.listen` is called. This function needs to retrieve the data for the current counter value. It can get the data snapshot (which is a copy of data at the database) from the event first, and then retrieve the concrete value.

This new value goes into the `count` property of our app, and AngularDart takes care of updating the view.

The `ngOnInit(`) method now looks like this:
```cs
@override
ngOnInit() {
  initializeApp(
      apiKey: "AIzaSyAH7S_gsce9RtNI8w0z7doiP3ugVJM8ZbI",
      authDomain: "angulardart-firebase-io-2017.firebaseapp.com",
      databaseURL: "https://angulardart-firebase-io-2017.firebaseio.com",
      storageBucket: "angulardart-firebase-io-2017.appspot.com");

  ref = database().ref('counter');
  ref.onValue.listen((e) {
    count = e.snapshot.val();
  });
}
```
▶️  Refresh the app in the web browser. You should now see a nonzero value there:

![](https://codelabs.developers.google.com/codelabs/angulardart-firebase-web-app/img/6a2d6befa3734b06.png)

The value you see is probably different, because it depends on the actual value in the Firebase database.

> If the app doesn't load and the console shows FirebaseJsNotLoadedException, make sure your `web/index.html` file correctly includes the **firebase.js** script.

## [**`SEE THE SOURCE CODE`**](https://github.com/Janamou/firebase-counter-steps/tree/master/4-loaddatafromfirebase/firebase_counter)

In the next step, you'll add more interactivity to the counter.




