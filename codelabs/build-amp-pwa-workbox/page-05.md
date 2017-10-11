# 5. Cache dynamic content

You just took the first step towards transforming your AMP site into a PWA by creating and installing a service worker that caches static content. Now we want to cache dynamic content (such as each AMP page) when it's loaded.

## Cache visited pages

To cache pages the user has visited, we must add a route to the service worker.

Add the following code to the bottom of **src/sw.js**:

### src/sw.js
```javascript
workboxSW.router.registerRoute('/*', args => {
  if (args.event.request.mode !== 'navigate') {
    return workboxSW.strategies.cacheFirst().handle(args);
  }
  return workboxSW.strategies.networkFirst().handle(args);
});
```
Save the file.

### Explanation

The `registerRoute` method takes a URL pattern and a callback to handle requests matching that pattern. This particular route intercepts all requests, and uses the [`request.mode`](https://developer.mozilla.org/en-US/docs/Web/API/Request/mode) attribute to check if the request is for a navigation (to another page). If the request is for a navigation, a network-first caching strategy is used. If the request is not a navigation (such as an image request), a cache-first strategy is used. Both of these built-in Workbox strategies dynamically cache resources when they are loaded. (You can learn more about [`Regular Expressions`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions) here.)

> **Tip**: When implementing a caching strategy for your pages, make sure to use a strategy that properly fits your business needs. Workbox has [several caching strategies](https://workboxjs.org/reference-docs/latest/module-workbox-runtime-caching.html) available, and it's also possible to mix and match to create your own, as in the example above.

## Cache the AMP runtime

Our AMP pages still haven't been completely configured for offline viewing. So far, our code caches the pages and supporting content, but we still need to configure the AMP runtime to be cached.

Append the following route to **src/sw.js**:

### src/sw.js
```javascipt
workboxSW.router.registerRoute(/(.*)cdn\.ampproject\.org(.*)/,
  workboxSW.strategies.staleWhileRevalidate()
);
```

### Explanation
This route caches the AMP runtime using the `staleWhileRevalidate` strategy. This strategy requests the resource from the cache and network in parallel, and then responds with the cached version. When the network request completes, the cache is updated with the newest version of the AMP runtime, so that it can be served on the next request.

## Cache a custom offline page
Now, when a visitor goes to a previously visited page while offline, they see a cached version of the page. But if the user clicks a link that wasn't previously visited, they see the browser's default offline page. By using a service worker, it's possible to serve a customized offline page.

In **project**, create a file called **offline.html** and add this code:

### offline.html
```html
<!doctype html>
<html>
<head>
  <meta charset="utf-8">
  <title>The Photo Blog - Offline</title>
  <meta name="viewport"
        content="width=device-width,minimum-scale=1,initial-scale=1">
</head>
<body>
  <h1>You are Offline</h1>
</body>
</html>
```

In **workbox-cli-config.js**, add **offline.html** to the list of static files to cache in the `staticFileGlobs`:

### workbox-cli-config.js
```javascript
module.exports = {
  "globDirectory": "./",
  "globPatterns": [
    "img/**.*",
    "offline.html"
  ],
  "swSrc": "src/sw.js",
  "swDest": "service-worker.js",
  "globIgnores": [
    "./workbox-cli-config.js"
  ]
};
```

In **src/sw.js**, add a `.then` after the `workboxSW.strategies.networkFirst().handle(args)` method call to return the offline page when a request fails. The full **src/sw.js** should look like this:

### src/sw.js
```javascript
importScripts('workbox-sw.dev.v2.0.0.js');

const workboxSW = new self.WorkboxSW();
workboxSW.precache([]);

workboxSW.router.registerRoute('/*', args => {
  if (args.event.request.mode !== 'navigate') {
    return workboxSW.strategies.cacheFirst().handle(args);
  }
  return workboxSW.strategies.networkFirst().handle(args).then(response => {
    if (!response) {
      return caches.match('offline.html');
    }
    return response;
  });
});

workboxSW.router.registerRoute(/(.*)cdn\.ampproject\.org(.*)/,
  workboxSW.strategies.staleWhileRevalidate()
);
```

### Explanation
The `networkFirst().handle()` method in our handler returns a promise that resolves with the response. If the network is unavailable and the requested resource is not found in the cache (because the user never visited the corresponding page), the promise resolves to `undefined`. This is a perfect time to serve up our custom offline page instead. In this case, if `response` is `undefined` then **offline.html** is returned from the cache.

> **Tip:** The offline page is a regular HTML page, so it's possible to use any kind of web technology, as long as the resources that the page depends on are cached.

## Test it out
Regenerate the service worker by running:

```
workbox inject:manifest
```

Return to the browser and refresh the page. Update the service worker in developer tools from the **Application > Service Workers** tab by clicking **skipWaiting**.

![](https://codelabs.developers.google.com/codelabs/amp-pwa-workbox/img/4e44e2da530c44ae.png)

Now refresh the page again and visit an article. This loads and caches the resources for these pages. From the **Application** tab, right click **Cache Storage** and click **Refresh Caches**. Expand **ache Storage** and check the caches to verify that the files necessary for the page are listed in the cache.

![](https://codelabs.developers.google.com/codelabs/amp-pwa-workbox/img/ef2f93b70b539298.png)

In the **Service Workers** section, click the **Offline** checkbox and reload the page. You should see that the page loaded even though it's offline. Now, navigate back to the home page and click one of the links to an unvisited article. Because the article wasn't cached, you should see the custom offline page.

> Tip: If you're seeing different results, try going to the **Clear Storage** section, making sure all checkboxes are checked and clicking **Clear Selected**, and then try reloading the page (remembering to uncheck **Offline** mode). This removes the service worker, clears all caches, and ensures the behavior described above.













