# 8. (Optional) Browsers that don't support service workers

In the current implementation, when the user navigates the site on a browser that doesn't support service workers, they never see the app shell experience. Fortunately, the `amp-install-serviceworker` component provides a [fallback](https://www.ampproject.org/docs/reference/components/amp-install-serviceworker#shell-url-rewrite) that rewrites links on the page to the shell URL.

On the AMP pages, update the `amp-install-serviceworker` component to include the fallback attributes:

#### index.html, 1.html, 2.html, 3.html
```html
<amp-install-serviceworker 
    src="/service-worker.js" 
    layout="nodisplay"
    data-iframe-src="/install-service-worker.html"
    data-no-service-worker-fallback-url-match=".*"
    data-no-service-worker-fallback-shell-url="/shell.html">
  </amp-install-serviceworker>
```

Now, when the user is using a browser that doesn't support service workers and clicks a link inside the AMP files, the AMP runtime replaces the link with the shell URL and appends the original URL as a fragment, in the format `#href=<original url>`.

So, a link to **/articles/1.html** becomes **/shell.html#%2Farticles%2F1.html**.

Let's make our app shell's code aware of this change. Add a function to find out the correct URL for the content and replace `const url = document.location.href;` with a call to that function:

### app.js
```javascript
function getContentUri() {
  const hash = window.location.hash;
  if (hash && hash.indexOf('href=') > -1) {          
    return decodeURIComponent(hash.substr(6));
  }
  return window.location;  
}

...

const ampRoot = document.querySelector('#amproot');
// const url = document.location.href;
const url = getContentUri();
const amppage = new AmpPage(ampRoot, router);
```

Now when a user visits your AMP page and clicks a link, they'll be taken to the app shell. However, when inside the shell, links to other pages won't point to the shell. We have to replace these links "manually" in the shell's JavaScript.

Update the `replaceLinks` function with the following code:

### app.js
```javascript
replaceLinks(document) {
  if ('serviceWorker' in navigator) {
    return;
  }
  const elements = document.getElementsByTagName('a');
  for (let i = 0; i < elements.length; i++) {
    const anchor = elements[i];
    const href = anchor.href;
    anchor.href = '/shell.html#href=' + encodeURIComponent(href);
  }
}
```

Finally, add a call to `router.replaceLinks(document)` just after the `router` object is instantiated:

### app.js

```javascript
const ampReadyPromise = new Promise(resolve => {
  (window.AMP = window.AMP || []).push(resolve);
});
const router = new Router();
// here
router.replaceLinks(document);
```

This updates the links in the shell itself so that they point back to the shell. In this case it's just the one link to the AMP home page (**index.html**).

Then update the `loadDocument` function to replace the links as well:

### app.js
```javascript
loadDocument(url) {
    return this._fetchDocument(url).then(document => {
      router.replaceLinks(document);
      const header = document.querySelector('.header');
      header.remove();
      window.AMP.attachShadowDoc(this.rootElement, document, url);
    });
  }
```
This updates the links inside the AMP pages as they're injected into the shell.

### Explanation

If the user is not coming from the service worker, the URL fragment will be present, so we can extract the final URL using `decodeURIComponent`. If the content of the URL was replaced by the service worker, we already have the correct URL of the AMP page.

## Fixing broken URLs

A side effect of using the URL rewrite fallback is that links to resources are broken and the URL in the URL bar shows the shell URL with the fragment. We can use the Page History API to fix it.

Update the `ampReadyPromise` chain with the following code:

#### app.js
```javascript
ampReadyPromise.then(() => {
  return amppage.loadDocument(url);
}).then(() => {
  if (window.history) {
    window.history.replaceState({}, '', url);
  }
});
```

Once the `loadDocument` Promise successfully resolves, we replace the shell URL with the one that was just loaded.

## Test it out
Start a browser that [doesn't support service worker](https://jakearchibald.github.io/isserviceworkerready/#service-worker-enthusiasm). At the time of this writing, browsers you might try include Edge on Windows and Safari on OSX. Open the web page. The first view should be an AMP page. To verify this, open Developer Tools in the browser and view the source code (or observe the absence of the "This is the app shell!" message at the bottom of the page). Now, click one of the links on the initial page and notice that the URL was rewritten. Also, notice how the page being rendered uses the app shell.


## Congratulations!
You've just finished implementing your first AMP in a PWA. Your application has all the advantages of the AMP as a canonical PWA pattern, and you can now add features that were only possible in PWA.

## What's Next?
This codelab only scratches the surface with regards to what is possible when building PWAs with AMP. When building a more complex experience, be sure to check the application design against the [PWA Checklist](https://developers.google.com/web/progressive-web-apps/checklist). 

Looking for more? Check out Paul Bakaus's [Progressive Web AMPs](https://www.smashingmagazine.com/2016/12/progressive-web-amps/) article.


