# 4. Install the Firebase Command Line Interface

The Firebase Command Line Interface (CLI) will allow you to serve the web app locally and deploy your web app and Cloud Functions.

> To install the CLI you need to have installed [npm](https://www.npmjs.com/) which typically comes with [NodeJS](https://nodejs.org/en/).

To install or upgrade the CLI run the following npm command:

```
npm -g install firebase-tools
```

> Doesn't work? You may need to [change npm permissions.](https://docs.npmjs.com/getting-started/fixing-npm-permissions)

To verify that the CLI has been installed correctly, open a console and run:

```
firebase --version
```

Make sure the Firebase version is above **3.5.0** so that it has all the latest features required for Cloud Functions. If not, run `npm install -g firebase-tools` to upgrade as shown above.

Authorize the Firebase CLI by running:

```
firebase login
```

Make sure you are in the `cloud-functions-start` directory then set up the Firebase CLI to use your Firebase Project:

```
firebase use --add
```

Then select your Project ID and follow the instructions. When prompted, you can choose any Alias, such as `codelab` for instance.