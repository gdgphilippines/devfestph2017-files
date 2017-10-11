# 13. Add material components

In this step, you add material design buttons with icons to your app. There is no need to change the logic; only the button component needs to change.

The material design library is already present in the `pubspec.yaml` file's dependencies section:
```
dependencies:
  ...
  angular_components: ^0.6.0
```
The `AppComponent` already contains the needed material design providers and directives.
```javascript
@Component(
  ...
  directives: const [materialDirectives, TodoListComponent],
  providers: const [materialProviders],
)
class AppComponent implements OnInit {
```
The only thing you need to do is use the [material components—specifically](https://dart-lang.github.io/angular_components_example/#Buttons), the material floating action button (fab) with icon.

▶️  Change your template code in `lib/app_component.html` :

1. Use `<material-fab>` components instead of `<button>` elements.
2. Use `trigger` events instead of `click` events.
3. Put a `<material-icon`> component inside of each `<material-fab>` component, instead of text.

Your code should look like this:
```html
<material-fab (trigger)="dislike()">
    <material-icon icon="thumb_down"></material-icon>
</material-fab>

<span>{{count}}</span>

<material-fab (trigger)="like()">
    <material-icon icon="thumb_up"></material-icon>
</material-fab>
```

The advantage of using `trigger` instead of `click` is in accessibility — the `trigger` event combines clicks, keyboard events, and taps.

The `<material-icon>` components have an `icon` attribute that's set to `thumb_down` or `thumb_up`. You can find icons and their corresponding names at [material.io/icons](https://material.io/icons/). This app loads its icons using a web font that's already included in `web/styles.css`:

![](https://codelabs.developers.google.com/codelabs/angulardart-firebase-web-app/img/60dea22f64a2d25a.png)

Finally let's add some styling to our component.

▶️  Update stylesheets for your app component in `lib/app_component.css` to be:
```css
span {
 font-size: 300%;
 padding: 0 1rem;
}
```

▶️  Refresh the app in the web browser.

![](https://codelabs.developers.google.com/codelabs/angulardart-firebase-web-app/img/f9c89c346f3306b4.png)

You should see the material design buttons in action. And that's it!

## [**`SEE THE SOURCE CODE`**](https://github.com/Janamou/firebase-counter-steps/tree/master/6-addmaterialcomponents/firebase_counter)

or

## [**`TRY IT`**](https://janamou.github.io/firebase-counter/)







