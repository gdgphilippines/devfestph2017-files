# 7. Import the Cloud Functions and Firebase Admin modules

Two modules will be required during this codelab, the `firebase-functions` module allows us to write the Cloud Functions trigger rules, while the `firebase-admin` module allows us to use the Firebase platform on a server with admin access, for instance to write to the Realtime Database or send FCM notifications.

In the `index.js` file, replace the first `TODO` with the following:

#### index.js

```javascript
/**
 * Copyright 2017 Google Inc. All Rights Reserved.
 * ...
 */

// Import the Firebase SDK for Google Cloud Functions.
const functions = require('firebase-functions');
// Import and initialize the Firebase Admin SDK.
const admin = require('firebase-admin');
admin.initializeApp(functions.config().firebase);

// TODO(DEVELOPER): Write the addWelcomeMessage Function here.

// TODO(DEVELOPER): Write the blurImages Function here.

// TODO(DEVELOPER): Write the sendNotification Function here.
```

The Firebase Admin SDK can be configured automatically when deployed on a Cloud Functions environment. This is what we do above using `admin.initializeApp(functions.config().firebase);`

Now let's add a Function that runs when a user signs in for the first time in your chat app and we'll add a chat message to welcome the user.