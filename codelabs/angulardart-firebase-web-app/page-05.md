# 5. Your first component

In this step, you implement a simple AngularDart component for your counter.

AngularDart makes it easy to split apps into many small, independent components. But for our purposes, one component is enough for the whole app.

▶ In the left-hand panel, open the project folder and the **lib** folder inside it. Then double-click **app_component.dart**.

![](https://codelabs.developers.google.com/codelabs/angulardart-firebase-web-app/img/4444e3726d1cd3ad.png)

The `app_component.dart` file holds the source code for our main component, the `AppComponent` class. Think about it as the app itself.

The starter app doesn't have any state in the main component, but our counter app will. Let's add it.

▶️  Edit the inside of the AppComponent class to look like this:
```javascript
...
class AppComponent {
  int count = 0;
}
```
We merely removed the comment, substituting a single field (`count`) that we initialized to `0`.

▶️ Open `lib/app_component.html`, the HTML template for our component, and change the contents of the file to be only this:
```javascript
{{count}}
```

> The special `{{ }}` syntax defines an **interpolation** binding expression. At runtime, Angular replaces `{{count}}` with the value of the component's count property.

Refresh the app in the web browser, either by reloading the page or by clicking the Debug button again. You should now see the current value of our counter (0):

![](https://codelabs.developers.google.com/codelabs/angulardart-firebase-web-app/img/1575cb1bbe7a9f1e.png)

## [**`SEE THE SOURCE CODE`**](https://github.com/Janamou/firebase-counter-steps/tree/master/3-yourfirstcomponent/firebase_counter)

### What does the app_component.dart source code mean?

The `lib/app_component.dart` file imports the angular and angular_components libraries as well as `todo_list_component.dart`, which implements a component from the sample project. We don't use that component, but you can leave it there as-is.

The `@Component` annotation on `AppComponent` specifies the value of several parameters:

- **selector**: the CSS selector for the component. Angular creates and displays an instance of `AppComponent` when it encounters a `<my-app>` element in the HTML.
- **styleUrls**: a list of CSS stylesheets for the component.
- **templateUrl**: a template used along with the component. The template specifies HTML to be inserted wherever the `<my-app>` element appears in the app.
- directives: a list of directives and other components for the current component. Because this app uses material design components, `materialDirectives` is in this list.
- **providers**: a list of providers for the component. Because this app uses material design components, `materialProviders` is in this list.
Next, we'll modify the app to load data from the Firebase database.

Next, we'll modify the app to load data from the Firebase database.


