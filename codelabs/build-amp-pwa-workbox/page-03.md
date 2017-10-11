# 3. Cache the static resources

## Write the source service worker

Let's start by using `workbox-sw` to write a source service worker. Add the following code to __src/sw.js__:

### src/sw.js
```javascript
importScripts('workbox-sw.dev.v2.0.0.js');

const workboxSW = new self.WorkboxSW();
workboxSW.precache([]);
```

### Explanation
This code imports the `workbox-sw` library, and creates an instance of `WorkboxSW` so the service worker can access [the library methods](https://workboxjs.org/reference-docs/latest/module-workbox-sw.WorkboxSW.html#main) from this object.

The `precache` method takes a manifest of URLs to cache on service worker installation. `precache` intelligently updates the cache and sets up a fetch handler to respond cache-first for requests to URLs in the manifest. In the next step you use `workbox-cli` to generate the manifest automatically. You can learn more about Workbox in [this lab](https://developers.google.com/web/ilt/pwa/lab-workbox).

## Build the production service worker

When using Workbox, it is recommended that you generate the list of files to precache using one of the Workbox build modules. This allows Workbox to create hashes of your files and intelligently update the caches when you edit your files.

Create a file in __project__ and name it __workbox-cli-config.js__. Add the following content to the file:

### workbox-cli-config.js
```javascript
module.exports = {
  "globDirectory": "./",
  "globPatterns": [
    "img/**.*"
  ],
  "swSrc": "src/sw.js",
  "swDest": "service-worker.js",
  "globIgnores": [
    "./workbox-cli-config.js"
  ]
};
```

Now, run `workbox-cli` from the project directory:
```javascript
workbox inject:manifest
```

> __Note:__ If the development server is blocking the command line, open a new command line tab or window.

Open __project/service-worker.js__ and observe that __img/amp_logo_white.svg__ has been added to the precache call.


### Explanation

workbox-cli uses the settings in the config file to build a new service worker file. The inject:manifest command copies the source service worker (src/sw.js) to the destination service worker (service-worker.js), and adds the list of files matched by the globPatterns to the precache method in the destination service worker. After this service worker is installed, it precaches the globPatterns resources. We install the service worker in the next step.

> __Note:__ Run the `workbox inject:manifest` command to regenerate the script every time an image is added or changed.





