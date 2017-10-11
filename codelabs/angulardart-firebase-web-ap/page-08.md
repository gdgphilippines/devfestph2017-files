# 8. Initialize the Firebase app

▶️  In `lib/app_component.dart`:

1. Add an `import` of Firebase library at the top:
```javascript
import 'package:firebase/firebase.dart';
``` 
2. Declare that `AppComponent` implements the `OnInit` interface:
```javascript
class AppComponent implements OnInit {
```

Why? We want to initialize our connection and load our data from Firebase right after our app is instantiated. We'll do this in the `ngOnInit()` method. This method is one of the component's [lifecycle hooks](https://webdev.dartlang.org/angular/guide/lifecycle-hooks).

AppComponent is underlined. If you mouse over it, you'll see the message **"Missing concrete implementation of OnInit.ngOnInit"**.

▶️  Click the underlined **AppComponent** identifier, then click the red light bulb that appears and select **Create 1 missing override(s)**.

![](https://codelabs.developers.google.com/codelabs/angulardart-firebase-web-app/img/71fe2bb79b193e31.png)

Alternately, use the Alt-Enter keyboard shortcut when your cursor is in the underlined text.

The `ngOnInit()` method is created and your code should now look like this:
```javascript
class AppComponent implements OnInit {
  int count = 0;

  @override
  ngOnInit() {
    // TODO: implement ngOnInit
  }
}
```

▶️  Inside n`gOnInit()`, call the `initializeApp()` function with the configuration object for your Firebase project:
```json
ngOnInit() {
  initializeApp(
     apiKey: "AIzaSyAH7S_gsce9RtNI8w0z7doiP3ugVJM8ZbI",
     authDomain: "angulardart-firebase-io-2017.firebaseapp.com",
     databaseURL: "https://angulardart-firebase-io-2017.firebaseio.com",
     storageBucket: "angulardart-firebase-io-2017.appspot.com");
}
```
> The Firebase configuration is shared by all AngularDart + Firebase codelabs. To simplify getting started, we've already created a new project in the Firebase console, and we provide you the needed project configuration object.

You're done with the configuration but aren't doing anything with Firebase yet. That's the next step.





