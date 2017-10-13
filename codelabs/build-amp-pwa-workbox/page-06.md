# 6. Adding the app to the home screen

## Write the Web App Manifest

One of the ways a PWA resembles a native application is that it can be [added to the Home Screen](https://developer.chrome.com/multidevice/android/installtohomescreen). [Adding a Web Manifest](https://w3c.github.io/manifest/) enables this (on Chrome).

Writing a Web Manifest by hand can be an error prone task. There are several online manifest generators available on the web that can help with this task. For example, [https://app-manifest.firebaseapp.com/](https://app-manifest.firebaseapp.com/), which also generates icons for you. We have provided the manifest code and icons for convenience. Create a file called **manifest.json** in the **project** directory and copy in the following code:

### manifest.json
```json
{
  "name": "AMP PWA Codelab",
  "short_name": "AMP PWA Codelab",
  "theme_color": "#00796b",
  "background_color": "#00796b",
  "display": "standalone",
  "start_url": "/index.html",
  "icons": [
    {
      "src": "icons/icon-72x72.png",
      "sizes": "72x72",
      "type": "image/png"
    },
    {
      "src": "icons/icon-96x96.png",
      "sizes": "96x96",
      "type": "image/png"
    },
    {
      "src": "icons/icon-128x128.png",
      "sizes": "128x128",
      "type": "image/png"
    },
    {
      "src": "icons/icon-144x144.png",
      "sizes": "144x144",
      "type": "image/png"
    },
    {
      "src": "icons/icon-152x152.png",
      "sizes": "152x152",
      "type": "image/png"
    },
    {
      "src": "icons/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "icons/icon-384x384.png",
      "sizes": "384x384",
      "type": "image/png"
    },
    {
      "src": "icons/icon-512x512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}
```

Note that the icons referenced by the manifest already exist in the starter code, and there is no need to create them.

### Explanation
The manifest attributes configure how the OS displays the app.

- The `name` and `short_name` are used in UI presentation, such as the home screen launcher.
- The `background_color` is used in the splash screen when the app launches.
- The `theme_color` determines the color of various UI components associated with your app (e.g., the border color of your app in the App Launcher)
- Setting `display` to `standalone` removes the browser chrome, so that the app can provide the look and feel of a native app.
- The `icons` are needed for the home screen launcher and the splash screen. Here we've provided different sizes for different devices and resolutions.
- The `start_url` is the resource that is opened when the user clicks the home screen icon.

## Link the manifest

Since the user might start from any AMP page, we need to link to the manifest from each page.

Add a link tag to the head of each AMP page:

#### index.html, 1.html, 2.html, 3.html
```html
<link rel="manifest" href="/manifest.json">
```

For the Add to Home Screen prompt to activate, the URL referenced on `start_url` must always be available, even when the user is offline. We must precache `index.html` to ensure that it is added to the cache when the service worker is installed. While we're at it, we can precache the Add to Home Screen icons.

In **workbox-cli-config.js**, add **index.html** and the **icons** to the list of static files to cache in the `staticFileGlobs`. Also add a `templatedUrls` property, such that the complete file matches the following:

### workbox-cli-config.js
```javascript
module.exports = {
  "globDirectory": "./",
  "globPatterns": [
    "img/**.*",
    "offline.html",
    "index.html",
    "icons/**.*"
  ],
  "swSrc": "src/sw.js",
  "swDest": "service-worker.js",
  "globIgnores": [
    "./workbox-cli-config.js"
  ],
  "templatedUrls": {
    "/": ["index.html"]
  }
};
```

The `templatedUrls` option tells Workbox that our site responds to requests for slash with the contents of **index.html**, so we don't end up having to manage two separate precache entries. Run the following command to update the generated service worker:
```
workbox inject:manifest
```

> **Note:** If you edit precached files, Workbox takes care of updating the caches for you, but you must remember to rebuild the service worker each time you make a change. In practice, you want to make this part of your build process. See [workboxjs.org](https://workboxjs.org/) for the build tools supported by Workbox.


## Test it out

Return to the browser and refresh the page (remember to uncheck offline mode). On Chrome Developer Tools, navigate to the **Application** tab and check the **Manifest** item. The information contained in the Manifest should be displayed on the right side.

![](https://codelabs.developers.google.com/codelabs/amp-pwa-workbox/img/10c314274cc26177.png)

Activate the updated service worker by clicking **skipWaiting** in the **Application** tab as you've done before. Then refresh the caches and observe that **index.html**, /, and the manifest icons are now cached in the workbox-precaching-revisioned cache.

> **Tip:** Chrome on Android will show a request to Add to Home Screen banner depending on a few heuristics. To debug and check if your site can trigger the banner, it's possible to disable the engagement checks by navigating to chrome://flags/#bypass-app-banner-engagement-checks and clicking **Enable**.

**Optional:** You may have noticed that, although the **index.html** page has been precached, the images for the articles were not (though they are cached dynamically on navigation). Although it is possible to precache the content, the dynamic nature of the index page makes it harder to know which images to cache. A possible solution for this is to serve a fallback offline image, when the requested one is not available. This is an exercise for the reader to do on their own time. Check out the [`placeholder`](https://www.ampproject.org/docs/guides/responsive/placeholders) property of `amp-img` to get started.

### Explanation
Congratulations, you've just implemented your first AMP as Canonical PWA. You have made your application more resilient to offline scenarios, optimized loading performance by caching static assets and images and became eligible to be launched from the home screen!

On your own time, use [Chrome's remote debugging](https://developers.google.com/web/tools/chrome-devtools/remote-debugging/) to add our app to [your device's home screen](https://developers.google.com/web/fundamentals/engage-and-retain/app-install-banners/).

![](https://codelabs.developers.google.com/codelabs/amp-pwa-workbox/img/fe61743658af4243.png)





