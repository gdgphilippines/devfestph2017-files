# 7. Include the Firebase script tag

Before you use the Dart `firebase` library, you must add the JavaScript it depends on to your app.

▶️  Include the latest `firebase.js` file in your `web/index.html` file, just before the other scripts:
```html
<script src="https://www.gstatic.com/firebasejs/4.1.3/firebase.js"></script>
```
After adding the script, your index.html file should look like this:

```html
...
<script src="https://www.gstatic.com/firebasejs/4.1.3/firebase.js"></script>
<script defer src="main.dart" type="application/dart"></script>
<script defer src="packages/browser/dart.js"></script>
...
```

Now you are prepared to use Firebase in your Dart code.

