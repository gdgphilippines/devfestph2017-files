# 10. New Message Notifications

In this section you will add a Cloud Function that sends notifications to participants of the chat when a new message is posted.

Using [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) (FCM) you can send notifications to your users in a cross platform and reliable way. To send a notification to a user you need their FCM device token. The chat web app that we are using already collects device tokens from users when they open the app for the first time on a new browser or device. These tokens are stored in the Firebase Realtime Database under the `/fcmTokens/$deviceToken` path.

If you would like to learn how to get FCM device tokens on a web app you can go through the [Firebase Web Codelab](http://codelabs.developers.google.com/codelabs/firebase-web/).

### Send notifications

To detect when new messages are posted you'll be using the `functions.database().path().onWrite` Cloud Functions trigger which runs your code when a write to a given path of the Firebase Realtime Database is detected. Add the `sendNotifications` Function into your `index.js` file:

#### index.js

```javascript
// Sends a notifications to all users when a new message is posted.
exports.sendNotifications = functions.database.ref('/messages/{messageId}').onWrite(event => {
  const snapshot = event.data;
  // Only send a notification when a new message has been created.
  if (snapshot.previous.val()) {
    return;
  }

  // Notification details.
  const text = snapshot.val().text;
  const payload = {
    notification: {
      title: `${snapshot.val().name} posted ${text ? 'a message' : 'an image'}`,
      body: text ? (text.length <= 100 ? text : text.substring(0, 97) + '...') : '',
      icon: snapshot.val().photoUrl || '/images/profile_placeholder.png',
      click_action: `https://${functions.config().firebase.authDomain}`
    }
  };

  // Get the list of device tokens.
  return admin.database().ref('fcmTokens').once('value').then(allTokens => {
    if (allTokens.val()) {
      // Listing all tokens.
      const tokens = Object.keys(allTokens.val());

      // Send notifications to all tokens.
      return admin.messaging().sendToDevice(tokens, payload).then(response => {
        // For each message check if there was an error.
        const tokensToRemove = [];
        response.results.forEach((result, index) => {
          const error = result.error;
          if (error) {
            console.error('Failure sending notification to', tokens[index], error);
            // Cleanup the tokens who are not registered anymore.
            if (error.code === 'messaging/invalid-registration-token' ||
                error.code === 'messaging/registration-token-not-registered') {
              tokensToRemove.push(allTokens.ref.child(tokens[index]).remove());
            }
          }
        });
        return Promise.all(tokensToRemove);
      });
    }
  });
});
```

In the Function above we are first exiting if the function was triggered because of an update in the message as we only want to send notifications for new messages. Then we're gathering all users' device tokens from the Firebase Realtime Database and sending a notification to each of these.

Lastly we're removing the tokens that are not valid anymore. This happens when the token that we once got from the user is not being used by the browser or device for instance if the user has revoked access.

### **Deploy the Function**

The Function will only be active after you've deployed it. On the command line run `firebase deploy --only functions`:

```
firebase deploy --only functions
```

This is the console output you should see:

```
i  deploying functions
i  functions: ensuring necessary APIs are enabled...
✔  functions: all necessary APIs are enabled
i  functions: preparing functions directory for uploading...
i  functions: packaged functions (X.XX KB) for uploading
✔  functions: functions folder uploaded successfully
i  starting release process (may take several minutes)...
i  functions: updating function addWelcomeMessages...
i  functions: updating function blurOffensiveImages...
i  functions: creating function sendNotifications...
✔  functions[addWelcomeMessages]: Successful update operation.
✔  functions[blurOffensiveImages]: Successful updating operation.
✔  functions[sendNotifications]: Successful create operation.
✔  functions: all functions deployed successfully!

✔  Deploy complete!

Project Console: https://console.firebase.google.com/project/friendlychat-1234/overview
```

### Test the function

Once the function has deployed successfully you'll need to have a user that signs-in for the first time. If you have signed-in already with your account you can open the Firebase Console Authentication section and delete your account from the list of users.

1.  Open your app in your browser using the hosting URL (in the form of `https://<project-id>.firebaseapp.com`).
2.  If you sign-in the app for the first time make sure you allow notifications when prompted:
    ![Chrome Image 1](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/93ec8bf8a1fea7e1.png)
3.  Close the chat app tab or display a different tab: Notifications appear only if the app is in the background. If you would like to learn how to receive messages while your app is in the foreground have a look at [our documentation](https://firebase.google.com/docs/cloud-messaging/js/receive#handle_messages_when_your_web_app_is_in_the_foreground).
4.  Using a different browser (or an Incognito window), sign into the app and post a message. You should see a notification displayed by the first browser:
    ![Notification Image](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/1e9ca7256d5508b9.png)

> Note: if you attempt to adapt this codelab to reach a third-party service outside of Google, you need to switch to the a paid plan. See the [pricing page](https://firebase.google.com/pricing/) for more details.