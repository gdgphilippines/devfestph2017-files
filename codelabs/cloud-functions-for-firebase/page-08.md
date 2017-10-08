# 8. Welcome New Users

### Chat messages structure

Message posted to the FriendlyChat chat feed are stored in the Firebase Realtime Database. Let's have a look at the data structure we use for a message. To do this, post a new message to the chat that reads "Hello World":

![Chat App Image 1](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/f0578fea7cd44b48.png)

This should appear as:

![Chat App Image 2](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/97c41bfff6343749.png)

In your Firebase app console open the Realtime Database section:

![Realtime Database Image](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/f9d2e6dc58d298dd.png)

As you can see, chat messages are stored in the Realtime Database as an object with the `name`, `photoUrl` and `text` attributes added to the `/messages` collection.

### Adding welcome messages

The first Cloud Function adds a message that welcomes **new users** into the chat. For this we can use the trigger `functions.auth().onCreate` which runs the function every time a user signs-in for the first time in your Firebase app. Add the `addWelcomeMessages` function into your `index.js` file:

#### index.js

```javascript
// Adds a message that welcomes new users into the chat.
exports.addWelcomeMessages = functions.auth.user().onCreate(event => {
  const user = event.data;
  console.log('A new user signed in for the first time.');
  const fullName = user.displayName || 'Anonymous';

  // Saves the new welcome message into the database
  // which then displays it in the FriendlyChat clients.
  return admin.database().ref('messages').push({
    name: 'Firebase Bot',
    photoUrl: '/images/firebase-logo.png', // Firebase logo
    text: `${fullName} signed in for the first time! Welcome!` // Using back-ticks.
  });
});
```

Adding this function to the special `exports` object is Node's way of making the function accessible outside of the current file and is required for Cloud Functions.

In the function above we are adding a new welcome message posted by "Firebase Bot" to the list of chat messages. We are doing this by using the [`push`](https://firebase.google.com/docs/database/web/lists-of-data#append_to_a_list_of_data) method on the /`messages` collection in the Firebase Realtime Database which is where the messages of the chat are stored.

Since this is an asynchronous operation, we need to return the [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) indicating when the Realtime Database write has finished, so that Functions doesn't exit the execution too early.

#### Deploy the Function

The Function will only be active after you've deployed it. On the command line run `firebase deploy --only functions`:

```
firebase deploy --only functions
```

This is the console output you should see:

```
i  deploying functions
i  functions: ensuring necessary APIs are enabled...
⚠  functions: missing necessary APIs. Enabling now...
i  env: ensuring necessary APIs are enabled...
⚠  env: missing necessary APIs. Enabling now...
i  functions: waiting for APIs to activate...
i  env: waiting for APIs to activate...
✔  env: all necessary APIs are enabled
✔  functions: all necessary APIs are enabled
i  functions: preparing functions directory for uploading...
i  functions: packaged functions (X.XX KB) for uploading
✔  functions: functions folder uploaded successfully
i  starting release process (may take several minutes)...
i  functions: creating function addWelcomeMessages...
✔  functions[addWelcomeMessages]: Successful create operation. 
✔  functions: all functions deployed successfully!

✔  Deploy complete!

Project Console: https://console.firebase.google.com/project/friendlypchat-1234/overview
```

> Tip: The first time you are deploying functions for a project will take longer than usual because we're enabling APIs on your Google Cloud Project. The length of the deploy also depends on the number of functions being deployed and will increase as you add more over time.

#### Test the function

Once the function has deployed successfully you'll need to have a user that signs in for the first time.

1.  Open your app in your browser using the hosting URL (in the form of `https://<project-id>.firebaseapp.com`).
2.  With a new user, sign in for the first time in your app using the **Sign In** button.

  *   If you have already signed in the app you can open the [Firebase Console Authentication section](https://console.firebase.google.com/project/_/authentication/users) and delete your account from the list of users. Then sign in again.

  ![Firebase Console Authentication Image 1](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/824966d3fba2933b.png)

3.  After you sign in, a welcome message should be displayed automatically:

![Welcome Message Image 1](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/2613731a7255735c.png)

> Logs: If the welcome message doesn't appear or if you face any other issues further on, you should double-check the command line output for errors during the deploy. If that looks successful, visit the [Cloud Functions logs](https://console.firebase.google.com/project/_/functions/logs?search=&severity=DEBUG) to see if any error messages appeared. You can also verify that the function was deployed successfully and see how many times it has been executed by switching to the `DASHBOARD` tab.