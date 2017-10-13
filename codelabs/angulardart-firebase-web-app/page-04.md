# 4. Run the app

Let's run the starter app!

▶️  In WebStorm, click the **Debug**   ![](https://codelabs.developers.google.com/codelabs/angulardart-firebase-web-app/img/d7127d4e05d4632d.png)  button at the top-right of the window (it has ‘index.html' next to it).

> If you don't see the Debug icon or if it's greyed out, go to WebStorm's Project view (on the left), right-click `web/index.html`, and choose `Debug in browser > Dartium`. Alternately, drop into the terminal and run `pub serve` in the root of your project, then open the provided URL.

If this is the first time Dartium has executed after being downloaded, it opens a duplicate tab and displays messages for first-time Chromium users. You'll see warnings that you can safely ignore.

> If Chromium asks you to install the **JetBrains IDE Support** extension, do that. This enables debugging in WebStorm. Alternately, use the **Run ‘index.html'** (green arrow) button or menu item instead of Debug.

After a few seconds, you should see something like this:

![](https://codelabs.developers.google.com/codelabs/angulardart-firebase-web-app/img/2a7eb9e995c5381d.png)

## [**`SEE THE SOURCE CODE`**](https://github.com/Janamou/firebase-counter-steps/tree/master/2-setup/firebase_counter)

> If the browser displays `502 Bad Gateway`, then WebStorm probably didn't download the packages that the app needs. To fix this issue, find **pubspec.yaml** in the left panel, right-click it, and choose **Pub: Get Dependencies**. Then try to run the app again.

> Let's write some code for our first AngularDart component!






