# 3. Create a Firebase project and Set up your app

### Create project

In the [Firebase console](https://console.firebase.google.com) click on **CREATE NEW PROJECT** and call it **FriendlyChat**.

![Firebase Create Button Image](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/b956b992f90b2076.png)

> Note that while your project will be called **FriendlyChat** it will be assigned a unique ID in the form **friendlychat-1234** which is what is ultimately used to identify your project.

### Enable Google Auth

To let users sign-in on the web app we'll use Google auth, which needs to be enabled.

In the Firebase Console open the **Authentication** section > **SIGN IN METHOD** tab ([click here](https://console.firebase.google.com/project/_/authentication/providers) to go there) you need to enable the **Google** Sign-in Provider and click **SAVE**. This will allow users to sign-in the Web app with their Google accounts.

![FireBase Authentication Image](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/1e7a17d97761d124.png)