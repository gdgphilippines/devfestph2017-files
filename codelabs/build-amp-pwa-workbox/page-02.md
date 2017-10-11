# 2. Get set up

This lab requires Node.js. [Install](https://nodejs.org/en/) the latest long term support (LTS) version if you have not already done so.

Clone the starter code from GitHub with the following command:
```git
git clone https://github.com/googlecodelabs/amp-pwa-workbox.git
```

Alternatively, you can [click here to download the code](https://github.com/googlecodelabs/amp-pwa-workbox/archive/master.zip) as a zip file.

After you have the code, navigate to the __project__ directory via the command line:
```
cd amp-pwa-workbox/project/
```

## Starting a local server
A local development server is needed to test the code. If you already know how to run a development server, you can use your preferred methods. Otherwise, there are two options:


### Option 1. Node.js server
You can install a basic Node.js HTTP server with the following command:
```npm
npm install http-server -g
```

After it is installed, start the server from the project directory with the following command:
```
http-server -p 8081 -a localhost -c 0
```

You can terminate the server process using `Ctrl-c`.

## Option 2. Web Server for Chrome

You can install the [Web Server for Chrome browser extension](https://chrome.google.com/webstore/detail/web-server-for-chrome/ofhbbkphhbklhfoeikjpcbhemlocgigb?hl=en) on the Chrome Web Store. After installing it, click __Choose Folder__ to select the __project__ directory in the code repository. Set the port to 8081 and check the __Automatically show index.html__ option. (If the server is already running, you must switch it off and then on again to pick up the changes.)

![](https://codelabs.developers.google.com/codelabs/amp-pwa-workbox/img/171ce1612067d763.png)

## Open the app and explore the code

After you start a local server, open the browser and navigate to [http://localhost:8081/index.html](http://localhost:8081/index.html) to view the app. The app is made up of a set of AMP pages that link to each other. The goal of this lab is to make the app into a PWA.

> __Note:__ [Unregister](https://developers.google.com/web/ilt/pwa/tools-for-pwa-developers#unregister) any service workers and [clear all service worker caches](https://developers.google.com/web/ilt/pwa/tools-for-pwa-developers#clearcache) for localhost so that they do not interfere with the lab.

Open the __amp-pwa-workbox/project__ folder in your text editor. The __project__ folder is where you build the lab.

## Install project dependencies

This lab uses [Workbox](https://workboxjs.org/), a set of tools for writing service worker code. Install the `workbox-cli` module via npm by running the following command:

This lab also uses `workbox-sw` module, which is provided for convenience in __project/workbox-sw.dev.v2.0.0.js__. `workbox-sw` is a high-level library that makes it easier to precache assets and configure routes with caching strategies in a service worker.














