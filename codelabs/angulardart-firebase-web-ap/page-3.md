# 3. Create a new app

In this step, you create an AngularDart app, no coding required. WebStorm uses a template to provide all the files your app needs.

▶️  Launch WebStorm. ![](https://codelabs.developers.google.com/codelabs/angulardart-firebase-web-app/img/aceabefc4c9aca36.png)

▶️ In the **Welcome to WebStorm** window, click **Create New Project**. A **New Project** form appears.

▶️  Select **Dart** from the list on the left.

▶️  If the **Dart SDK path** and **Dartium path** fields don't have values, enter them. Make sure the Dart version is **1.24**.

▶️  Make sure that **Generate sample content** is checked. (It might take a few seconds to show up.)

▶️  Select **AngularDart Web App** from the list of sample content.

▶️  Change the name of the project in the **Location** input field from `untitled` to `firebase_counter`.

▶️  Check your settings against the following screenshot, then click **Create**.


Give WebStorm a few seconds, and new Angular web application project is loaded in WebStorm, including all dependencies.

![](https://codelabs.developers.google.com/codelabs/angulardart-firebase-web-app/img/c8b04ea23e2af203.png)

> WebStorm runs `pub get`, using Dart's **package management tool** to download all the needed packages (libraries) for the project. All package dependencies are written in the `pubspec.yaml` file.

> You might see some analysis errors until all the dependencies for your app are downloaded and analyzed.








