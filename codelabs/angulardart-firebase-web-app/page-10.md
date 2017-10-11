# 10. Add buttons

The application is a little bit _boring_ now. It shows the current value of counter from the Firebase Realtime Database, but it doesn't do anything special. Let's fix that.

In this step, you'll add like and dislike buttons to change the value in the shared counter.

▶️  Add two HTML buttons to the HTML template. Edit the `lib/app_component.html` file to be:
```html
<button>Dislike</button>
<span>{{count}}</span>
<button>Like</button>
```

You've added like and dislike buttons, but they don't do anything yet. To make them react, you need to register a `click` handler on them. The `click` handler specifies which function is called when the event is triggered.

> We also added <span> tags around {{count}}. It'll help us style the app later on.

▶️  Register click handlers in the template's HTML:

```html
<button (click)="dislike()">Dislike</button>
<span>{{count}}</span>
<button (click)="like()">Like</button>
```

> Notice that you registered event handlers using parentheses: `(click)="like()"`. In AngularDart, parentheses refer to output from the component (click events, for example).

▶️  Refresh the app in the browser.

The app stops at the loading screen and throws an exception: `NoSuchMethodError: Class 'AppComponent' has no instance getter 'dislike'`. This is great! It shows that AngularDart checks whether the methods used in the template actually exist. If they don't, that's definitely a bug we want to fix. And we don't need to click around our app to find out about it.

> If you don't see the exception in WebStorm, make sure the Debug console is visible. You can do that by clicking the icon in the bottom-left corner of WebStorm's window, then selecting **Debug**.

> ![](https://codelabs.developers.google.com/codelabs/angulardart-firebase-web-app/img/6278dd0c8e422c4e.png)

> If that doesn't help, make sure the JetBrains IDE Support extension is running. If it isn't, restart the session by clicking the **Debug**  button in WebStorm.

The problem is that we have refer to `dislike()` and `like()` functions for our buttons, but we haven't implemented them yet in our `AppComponent` class.

The `dislike()` method should decrease the counter's value; the `like()` method should increase it. Let's print something to console in our methods to try it.

▶️ Implement the `like()` and `dislike()` functions in `app_component.dart` like so:
```javascript
...
class AppComponent implements OnInit {
  ...
 
  dislike() {
    print("dislike");
  }
  
  like() {
    print("like");
  }
}
```
Test it in the web browser and watch the Debug console.

> If you close the "JetBrains IDE Support is debugging this tab" yellow warning at the top of Dartium, the console messages don't show up in WebStorm. To see them, either open Dartium's console instead, or restart the debug session by clicking **Debug(())  again. This reinstates the link between Dartium and WebStorm.

> Because the WebStorm debug console deduplicates log messages, clicking "like" many times in a row prints it only once.









