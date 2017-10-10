# 5. Deploy and run the web app

Now that you have imported and configured your project you are ready to run the web app for the first time. Open a console at the `cloud-functions-start` folder and run `firebase deploy --except functions` this will only deploy the web app to Firebase hosting:

```
firebase deploy --except functions
```

This is the console output you should see:

```
i deploying database, storage, hosting
✔  database: rules ready to deploy.
i  storage: checking rules for compilation errors...
✔  storage: rules file compiled successfully
i  hosting: preparing ./ directory for upload...
✔  hosting: ./ folder uploaded successfully
✔ storage: rules file compiled successfully
✔ hosting: 8 files uploaded successfully
i starting release process (may take several minutes)...

✔ Deploy complete!

Project Console: https://console.firebase.google.com/project/friendlychat-1234/overview
Hosting URL: https://friendlychat-1234.firebaseapp.com
```

### Open the web app

The last line should display the **Hosting URL.** The web app should now be served from this URL which should be of the form https://<project-id>.firebaseapp.com. Open it. You should see a chat app's functioning UI.

Sign-in to the app by using the **SIGN-IN WITH GOOGLE** button and feel free to add some messages and post images:

![Friendly Chat App Image](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/eb22bdc11ce003e0.png)

If you sign-in the app for the first time on a new browser make sure you allow notifications when prompted:
![Chrome Browser Image 1](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/93ec8bf8a1fea7e1.png)

We'll need you to have notifications enabled at a later point.

If you have accidentally clicked **Block** you can change this setting by clicking on the **🔒 Secure** button on the left of the URL in the Chrome Omnibar and selecting **Notifications > Always Allow on this Site**:

![Chrome Browser Image 2](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/4b309f3754532766.png)

Now we'll be adding some functionality using the Firebase SDK for Cloud Functions.