## 10. Extra Credit: Build the app for production, and deploy it to Firebase

### Build the app for production

Now you'll generate a **build** of your app. You'll package up the app into a format that you can push to an external-facing web server.

1. With a text editor, create a new file called `polymer.json`. Save it in the root `whose-flag` project folder. 
2. Paste the following text into `polymer.json`, and save the file:

```json
{
 "sources": [
   "data/*",
   "data/svg/*"
 ]
}
```
This makes sure that your app's data and images are included in the build.

Note: If you already have a `polymer.json` file in your project folder, delete its contents and replace them with the code sample above.

3. Type `polymer build`.

To see what polymer build has done, let's examine the folder structure again. This is what we had before:

![](https://codelabs.developers.google.com/codelabs/whose-flag/img/a68f46b93492d612.png)

4. In the main `/whose-flag/` folder, type `ls` to see the changes:
![](https://codelabs.developers.google.com/codelabs/whose-flag/img/bc70e2edf04a0a8f.png)

You'll notice the `build` folder. This folder contains the `default` folder, which in turn contains all of the files that your app needs in order to function "in the wild" on a web server. This is the stuff that you will deploy.

### Deploy your app with Firebase

Before you can deploy the build, you need to set up a place to host it. You can use any web server to deploy a Polymer app, but we're going to use [Firebase Hosting](https://firebase.google.com/), a web app hosting platform.

Setting up app hosting is a once-off step. In future, deploying your app will be even simpler!

#### Install Firebase tools
Firebase is an app hosting tool that you'll use to deploy your app on the Internet.

1. Use npm to install the Firebase command line tools:
```
npm install -g firebase-tools
```

2. To check that the Firebase tools installed correctly:
```
firebase --version
```

![](https://codelabs.developers.google.com/codelabs/whose-flag/img/e7474785f91d20bc.png)

### Set up app hosting with Firebase

1. [Click here to sign up for a Firebase account](https://www.firebase.com/signup/). If you have gmail, this is as simple as signing in to Firebase with your gmail address.
2. Once you've signed in to Firebase, from the [Firebase Console](https://console.firebase.google.com/), click **Create a project**.
3. Give your project a name, then click **Create Project**.
4. Firebase assigns your project an ID and redirects you to an overview of your project. In the URL, you'll see the ID your project has been assigned. Mine is `whose-flag`, but Firebase may add some random numbers to yours in order to make it unique.

![](https://codelabs.developers.google.com/codelabs/whose-flag/img/d0289b2572c642bd.png)

Take note of this project ID - you'll need it when you configure your app.

5. From the root `whose-flag` folder, type:
```
firebase login
firebase init
```

6. Firebase asks you which CLI features to set up. Make sure **Firebase Hosting** is selected. (Use the arrow keys to navigate the menu, and the Space bar to select or deselect an option.)

![](https://codelabs.developers.google.com/codelabs/whose-flag/img/22c32a05fc74c3a5.png)

7. Press **Enter**.
8. Select the project ID of the project you created in the Firebase Console, and press **Enter**.
9. Firebase asks you a set of questions about your app setup. Here's how to answer them:

-------
| Question | Answer |
-----------|--------|
| What file should be used for Database Rules? | `database.rules.json` |
| Do you want to install dependencies with npm now? | Y |
| What do you want to use as your public directory? | build/default |
| Configure as a single-page app (rewrite all urls to /index.html)? | N |
| File build/default/index.html already exists. Overwrite? | N | 

![](https://codelabs.developers.google.com/codelabs/whose-flag/img/99564c6ffe0d5109.png)

Done! Firebase has initialized a project for you. Let's look at the changes this command has made to your local app folder structure.

10. In the `/whose-flag/` folder, type `ls -a` (the `-a` flag shows hidden files).

![](https://codelabs.developers.google.com/codelabs/whose-flag/img/14038b9f56c30a87.png)


What are these new files and folders?

| File or folder |  What's inside? |
|----------------| ----------------|
| .firebaserc| Stores the name of your Firebase project. |
| database.rules.json | Stores the rules that define how Firebase should structure and control interaction with your app data. |
| firebase.json | Records some of the configuration decisions you just made. Importantly, this file stores the location of the built app (build/default) - the stuff that gets uploaded. |

### Deploy the build to your hosted Firebase project

Now, you need to deploy the build.

1. From the `/whose-flag/` folder, type `firebase deploy`.

![](https://codelabs.developers.google.com/codelabs/whose-flag/img/cd385a6692c8ef9c.png)

2. Visit the URL in the firebase deploy output. The URL will be based on the project ID that Firebase gave you in Step 4. For example, mine is this: https://whose-flag.firebaseapp.com