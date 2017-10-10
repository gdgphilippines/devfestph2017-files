# 6. Get the firebase package

In the next steps you'll use the Dart [firebase](https://pub.dartlang.org/packages/firebase) package, which provides a library that wraps the Firebase JavaScript SDK.

To use the firebase package, you first need to add it to your app's dependencies.

▶️  Open the `pubspec.yaml` file (in the root of your project).

▶️  Add `firebase: ^4.0.0` to the `dependencies` section.

```
dependencies:
  angular: ^4.0.0
  angular_components: ^0.6.0
  firebase: ^4.0.0
```

▶️  Save, then click **Get dependencies** at the top right of WebStorm's editor.

![](https://codelabs.developers.google.com/codelabs/angulardart-firebase-web-app/img/c730344bd3083f4a.png)

The `pub get` tool downloads the `firebase` package (and its dependencies) into your project.

> You can also download dependencies by going to the Project view, right-clicking **pubspec.yaml**, and choosing **Pub: Get Dependencies**.

Wait until the `firebase` dependency (with any other related dependencies) is loaded.

![](https://codelabs.developers.google.com/codelabs/angulardart-firebase-web-app/img/7734b07037109132.png)


