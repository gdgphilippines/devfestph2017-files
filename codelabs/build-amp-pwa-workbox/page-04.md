# 4. Install the service worker from AMP

Generating the service worker script is only half the task. The next step is installing the service worker from the AMP pages. AMP provides the [`amp-install-serviceworker`](https://www.ampproject.org/docs/reference/components/amp-install-serviceworker) component to this end.

Add the `amp-install-serviceworker` JavaScript to the end of the head section of each AMP page.

### index.html, 1.html, 2.html, 3.html
```html
<script async custom-element="amp-install-serviceworker" src="https://cdn.ampproject.org/v0/amp-install-serviceworker-0.1.js"></script>
```
Next, add the component configuration to the bottom of each AMP page, right before the `</body>` tag:


### index.html, 1.html, 2.html, 3.html
```html
<amp-install-serviceworker 
  src="/service-worker.js" 
  layout="nodisplay"
  data-iframe-src="/install-service-worker.html">
</amp-install-serviceworker>
```
Finally, write a file that registers the service worker. In **project**, create a **install-service-worker.html** file with the following content:

### install-service-worker.html
```html
<!doctype html>
<html>
  <head>
    <title>Installing service worker</title>
    <script type="text/javascript">
      if ('serviceWorker' in navigator) {
        navigator.serviceWorker.register('/service-worker.js')
          .then(function(reg) {
            console.log('SW scope: ', reg.scope);
          })
          .catch(function(err) {
            console.log('SW registration failed: ', err);
          });
      };
    </script>
  </head>
  <body>
  </body>
</html>
```

Save the file and refresh the page in the browser.

In Chrome Developer Tools, navigate to the **Application** tab and click **Service Workers**. You should see the service worker information, which looks similar to this:

![](https://codelabs.developers.google.com/codelabs/amp-pwa-workbox/img/82957da61fbd3f09.png)

You should also see that the **amp_logo_white.svg** file was cached. From the same **Application** tab, right-click **Cache Storage** and choose **Refresh Caches**. Expand **Cache Storage** to see the resource stored in the cache, similar to this:

![](https://codelabs.developers.google.com/codelabs/amp-pwa-workbox/img/5b16d4d4f5144a47.png)

### Explanation

In this step we added the [`amp-install-serviceworker`](https://www.ampproject.org/docs/reference/components/amp-install-serviceworker) component to each AMP page, and configured the component to install the production service worker (**service-worker.js**). AMP doesn't allow third party JavaScript directly, but the `amp-install-serviceworker` component has an extra attribute called `data-iframe-src` that loads a URL as an iframe. In this case, we've configured **install-service-worker.html** as the resource to be loaded, which registers and installs the service worker.


