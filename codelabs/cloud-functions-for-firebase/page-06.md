# 6. The Functions Directory

Cloud Functions allows you to easily have code that runs in the Cloud without a server.. We'll be showing you how to build functions that react to Firebase Auth, Cloud Storage and Firebase Realtime Database events. Let's start with Auth.

When using the Firebase SDK for Cloud Functions, your Functions code will live under the `functions` directory. Your Functions code is also a [Node.js](https://nodejs.org) app and therefore needs a `package.json` that gives some information about your app and lists dependencies.

To make it easier for you we've already created the `functions/index.js` file where your code will go. Feel free to inspect this file before moving forward.

```
cd functions
ls
```

If you are not familiar with [Node.js](https://nodejs.org) it will help to learn more about it before continuing the codelab.

The package.json file already lists two required dependencies: the [Firebase SDK for Cloud Functions](https://www.npmjs.com/package/firebase-functions) and the [Firebase Admin SDK](http://npmjs.com/package/firebase-admin). To install them locally run `npm install` from the `functions` folder:

```
npm install
```

Let's now have a look at the `index.js` file:

#### index.js

```javascript
/**
 * Copyright 2017 Google Inc. All Rights Reserved.
 * ...
 */

// TODO(DEVELOPER): Import the Cloud Functions for Firebase and the Firebase Admin modules here.

// TODO(DEVELOPER): Write the addWelcomeMessage Function here.

// TODO(DEVELOPER): Write the blurImages Function here.

// TODO(DEVELOPER): Write the sendNotification Function here.
```

We'll first import the required modules and then write three Functions in place of the TODOs. First let's import the required Node modules.