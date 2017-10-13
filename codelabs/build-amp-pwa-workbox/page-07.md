# 7. (Optional) Creating an app shell

Although the AMP as a Canonical PWA architecture may be enough in many scenarios, developers may need to use features that are not yet supported in AMP, such as Push Notifications or the Credentials Manager API.

For those scenarios, the AMP in a PWA architecture can be used.

## Replacing requests with the shell
Let's start by adding an app shell to our application. We then change our service worker, so that once installed, it can catch any navigation requests and replace them with the shell.

Create a file called **shell.html** in the **project** folder:

### shell.html
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <link rel="manifest" href="/manifest.json">
    <meta name="viewport" content="width=device-width,minimum-scale=1,initial-scale=1">  
    <title>AMP to PWA Demo</title>
    <style type="text/css">
body{margin:0;padding:0;background:#F5F5F5;font-size:12px;font-weight:300;font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,Oxygen-Sans,Ubuntu,Cantarell,"Helvetica Neue",sans-serif}a{text-decoration:none;color:#000}.header{color:#fff;background:#1976D2;padding:8px 16px;box-shadow:0 2px 5px #999;height:40px;display:flex;align-items:center}.header h1{margin:0 8px 0 0}.header amp-img{margin-right:8px}.header img{margin-right:8px}.header a{color:#fff}.header a:visited{color:#fff} 
    </style>
  </head>
  <body>
    <header class="header">
      <img src="/img/amp_logo_white.svg" width="36" height="36" />
      <h1><a href="/">AMP PWA Codelab - PWA</a></h1>
    </header>    
    <div id="amproot">
      <!-- AMP Content should appear here! -->
    </div>
    <h2>This is the app shell!</h2>
  </body>
</html>
```

Update the **workbox-cli-config.js** to precache the created **shell.html**, as well as **js/app.js**, which we add to the app shell in the next step. Here's how the full config should look:

### workbox-cli-config.js
```javascript
module.exports = {
  "globDirectory": "./",
  "globPatterns": [
    "img/**.*",
    "offline.html",
    "index.html",
    "icons/**.*",
    "shell.html",
    "js/app.js"
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

Update **src/sw.js** to serve **shell.html** for requests for the AMP pages. The full file should look like this:

### src/sw.js
```javascript
importScripts('workbox-sw.dev.v2.0.0.js');

const workboxSW = new self.WorkboxSW();
workboxSW.precache([]);

workboxSW.router.registerRoute('/*', args => {
  if (args.event.request.mode !== 'navigate') {
    return workboxSW.strategies.cacheFirst().handle(args);
  }
  return caches.match('/shell.html', {ignoreSearch: true});
});

workboxSW.router.registerRoute(/(.*)cdn\.ampproject\.org(.*)/,
  workboxSW.strategies.staleWhileRevalidate()
);
```
The main handler was changed, so that any request for an AMP page is replaced with the app shell. Now we need a way to inject AMP documents into the shell. We do that in the next step.

## Loading AMPs with amp-shadow
For this step, you will be using the JavaScript file located at **js/app.js** inside the **project** directory. This file already contains some boilerplate code needed for the shell to work: a method to fetch AMP documents from the backend and a Promise that allows scheduled code to be ran when the `amp-shadow` component is loaded.

To get started, add the `amp-shadow` component to the head section of the app shell:

### shell.html
```html
<!-- Asynchronously load the AMP-Shadow-DOM runtime library. -->
<script async src="https://cdn.ampproject.org/shadow-v0.js"></script>   
```

At the bottom of the **shell.html** `body`, import the **js/app.js** script:

### shell.html
```html
<script src="/js/app.js" type="text/javascript" defer></script>
```

Now, open the app.js script and add the following:

### app.js
```javascript
const ampRoot = document.querySelector('#amproot');
const url = document.location.href;
const amppage = new AmpPage(ampRoot, router);
ampReadyPromise.then(() => {
  amppage.loadDocument(url);
});
```

Then update the `loadDocument` function with code to fetch the AMP document and inject it into our `amproot` div:

### app.js
```javascript
loadDocument(url) {
  return this._fetchDocument(url).then(document => {
    window.AMP.attachShadowDoc(this.rootElement, document, url);            
  });       
}
```

Save the file.

### Explanation
The original request for the AMP page was replaced by the service worker with the content from **shell.html**, but the URL of the requested resource is still the same. When AMP is ready and `ampReadyPromis`e resolves, we load the originally requested AMP document (`document.location.href`), and inject it into the AMP root (`#amproot div`) in the shell using the `attachShadowDoc` method. This method injects the document using the Shadow DOM, which is a subtree of HTML elements that the browser includes in the rendering of a document, but doesn't expose in the main DOM tree. Check out Eric Bidelman's article [Shadow DOM v1: Self-Contained Web Components](https://developers.google.com/web/fundamentals/architecture/building-components/shadowdom) to learn more.

## Manipulating the content of the AMP file
When injecting the AMP pages into the app shell, it's likely that developers will add, remove, or change sections of the AMP document.

The fetched document is a regular DOM document, so a developer can manipulate it as needed. Update the `loadDocument` function with the following code:

### app.js
```javascript
loadDocument(url) {
  return this._fetchDocument(url).then(document => {
    const header = document.querySelector('.header');
    header.remove();
    window.AMP.attachShadowDoc(this.rootElement, document, url);            
  });       
}
```

### Explanation

The updated `loadDocument` function removes the header element from the AMP document, as the header is already present on the app shell and would otherwise be duplicated.

## Test it out

Rebuild the service worker with the following command:
```
workbox inject:manifest
```

Return to the browser, refresh the page, and activate the updated service worker with **skipWaiting**. The browser should render the AMP page, as the service worker has just activated. Reload the page again. The app shell version should now be rendered (you can verify by noting the "This is the app shell!" message at the bottom of the page, or inspecting the source code in Developer Tools).

> **Important:** Requests to AMP that result from a click on a link are caught by our service worker and replaced by the content of the shell.html file. But when the request is executed from the `_fetchDocument` method in **app.js**, the AMP file is fetched from the server. Wonder why this happens? It happens because, in our handler code, we only replace requests where `request.mode === 'navigate'`. When a request is run using `XHRHttpRequest` or `fetch`, it has a different mode.









