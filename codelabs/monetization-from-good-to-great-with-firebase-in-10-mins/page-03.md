# 3. Set up Firebase Console Project

### Create Firebase Console Project

1.  Go to the [Firebase console](https://console.firebase.google.com/).
2.  Select **Add Project**, and name your project "MonetizationCodelab".

### Connect your Android app

1.  From the overview screen of your new project, click **Add Firebase to your Android app**.
2.  Enter the codelab's package name: `com.google.firebase.codelab.monetizationcodelab`
3.  Set a nickname for your app: MonetizationCodelab.
4.  Leave the SHA-1 field blank since SHA-1 is not required for this project.
5.  Select **Register App** to register your app.

### Add google-services.json file to your app

Next, you will be prompted a screen where you can download a configuration file that contains all the necessary Firebase metadata for your app. Click **Download google-service.json** and copy the file into the _app_ directory in your project.

![Firebase for Android Image](https://codelabs.developers.google.com/codelabs/firebase-monetization/img/32419a0fa25a1405.png)

### Add google-services plugin to your app

The google-services plugin uses the google-services.json file to configure your application to use Firebase. The following line should already be added to the end of the build.gradle file in the _app_ directory of your project (check to confirm):

```javascript
apply plugin: 'com.google.gms.google-services'
```

### Sync your project with gradle files

To be sure that all dependencies are available to your app, you should sync your project with gradle files at this point. Select **Sync Project with Gradle Files** (<img src="https://codelabs.developers.google.com/codelabs/firebase-monetization/img/55eb8ab81b224812.png" alt="Gradle Sync Icon" width="20" height="20" class="small"/>) from the Android Studio tool bar.