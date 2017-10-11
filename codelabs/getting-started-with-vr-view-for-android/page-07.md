# 7. Load the image asynchronously

Since the image is large we don't want to load it in the main UI thread when the application starts. We want to load it asynchronously. For more details about image loading best practices see: [http://developer.android.com/training/displaying-bitmaps/index.html](http://developer.android.com/training/displaying-bitmaps/index.html).

### Create a new class

Select the app/java folder in the project outline, and then right mouse click and select **New > Java Class**. Name the class `com.google.devrel.vrviewapp.ImageLoaderTask`.

> **Note**: If you're prompted to add the file to Git, you can say no.

It needs to extend the AsyncTask class so it can perform the image loading in a background thread. The parameters to the AsyncTask are:

*   AssetManager to use to load the image.
*   Void parameters to the progress method (which we don't implement).
*   Bitmap return value back to the main thread.

Change the class declaration to be:

```java
public class ImageLoaderTask extends AsyncTask<AssetManager, Void, Bitmap>  {
}
```

Make sure the classes are imported:

```java
import android.content.res.AssetManager;
import android.graphics.Bitmap;
import android.os.AsyncTask;
```

There still is a problem. AsyncTask is an abstract class, so we need to implement the abstract methods.

> **Pro Tip**: To quickly resolve this error in Android Studio, click on the line with the error and press ⌥ +⏎ (The option key and return) and choose Implement Methods.

Add a skeleton implementation of doInBackground at the bottom of the class to resolve the error.

```java
@Override
protected Bitmap doInBackground(AssetManager... params) {
    return null;
}
```

### Add member variables and constructor

When we load the image, we need to pass which image to load and where to display it. We'll pass this information via the constructor. Also add a TAG member so we can log errors. Just below the opening brace of the class add:

```java
private static final String TAG = "ImageLoaderTask";
private final String assetName;
private final WeakReference<VrPanoramaView> viewReference;
private final VrPanoramaView.Options viewOptions;
```

We use a [WeakReference](http://developer.android.com/reference/java/lang/ref/WeakReference.html) for the VrPanoramaView since the view could be destroyed while loading the image. A common cause of this is rotating the phone to another orientation. By using a weak reference, the object can be garbage collected immediately instead of waiting for this async task to be destroyed.

Make sure to import the class for VrPanoramaView. If you have not already added them, add the import statements now:

```java
import com.google.vr.sdk.widgets.pano.VrPanoramaView;
import java.lang.ref.WeakReference;
```

To avoid re-loading the image when the device is rotated, we'll cache the last image loaded. We do this by keeping the name of the last asset loaded, and the resulting bitmap. Add these member variables:

```java
private static WeakReference<Bitmap> lastBitmap = new WeakReference<>(null);
private static String lastName;
```

Since these member variables are final, they need to be initialized in the constructor. Add the required constructor below the member variables:

```java
public ImageLoaderTask(VrPanoramaView view, VrPanoramaView.Options viewOptions, String assetName) {
    viewReference = new WeakReference<>(view);
    this.viewOptions = viewOptions;
    this.assetName = assetName;
}
```

### Load the image in the background

For this example, we've added the image to the `assets` directory of the project, so we'll use the `AssetManager` to get an `InputStream` to the image. Then pass that input stream to the `BitmapFactory` to load the image and return it back to the main thread. If there is a problem, we'll log it and return a `null` image. We check the last image loaded before opening the stream in order to conserve memory usage.

Replace the contents of `doInBackground(AssetManager... params)`with:

```java
AssetManager assetManager = params[0];

if (assetName.equals(lastName) && lastBitmap.get() != null) {
    return lastBitmap.get();
}

try(InputStream istr = assetManager.open(assetName)) {
    Bitmap b = BitmapFactory.decodeStream(istr);
    lastBitmap = new WeakReference<>(b);
    lastName = assetName;
    return b;
} catch (IOException e) {
    Log.e(TAG, "Could not decode default bitmap: " + e);
    return null;
}
```

Then add the import statements at the top:

```java
import android.graphics.BitmapFactory;
import android.util.Log;

import java.io.IOException;
import java.io.InputStream;

```

### Display the image

Back in the main thread, we can render the bitmap in the VrPanoramaView. Once the background work is done, the onPostExecute(Bitmap bitmap) method is called by AsyncTask on the main thread. In here, we'll pass the bitmap to the VrPanoramaView.

Just below `doInBackground()`, add:

```java
@Override
protected void onPostExecute(Bitmap bitmap) {
    final VrPanoramaView vw = viewReference.get();
    if (vw != null && bitmap != null) {
        vw.loadImageFromBitmap(bitmap, viewOptions);
    }
}
```

### Start the background loading

OK - so now that we have an AsyncTask to load and display the image, all that is left is calling it.

Go back to `WelcomeFragment.java.` We need a new member variable, so at the top of the class add

```java
private ImageLoaderTask backgroundImageLoaderTask;
```

Then at the bottom of the class add a new method **loadPanoImage().** This will create a new loader task and start it.

```java
private synchronized void loadPanoImage() {
    ImageLoaderTask task = backgroundImageLoaderTask;
    if (task != null && !task.isCancelled()) {
        // Cancel any task from a previous loading.
        task.cancel(true);
    }

    // pass in the name of the image to load from assets.
    VrPanoramaView.Options viewOptions = new VrPanoramaView.Options();
    viewOptions.inputType = VrPanoramaView.Options.TYPE_STEREO_OVER_UNDER;

    // use the name of the image in the assets/ directory.
    String panoImageName = "converted.jpg";

    // create the task passing the widget view and call execute to start.
    task = new ImageLoaderTask(panoWidgetView, viewOptions, panoImageName);
    task.execute(getActivity().getAssets());
    backgroundImageLoaderTask = task;
}
```

Finally, we need to tie it to a lifecycle event to start the actual loading. In a fragment we'll start it in `onActivityCreated()`. Add a new function at the end of the WelcomeFragment class:

```java
@Override
public void onActivityCreated(@Nullable Bundle savedInstanceState) {
    super.onActivityCreated(savedInstanceState);
    loadPanoImage();
}
```

### Save the file and try out the VR view image!

Make sure your device is attached, and click Run. Hold the phone upright and as you turn around the view changes as if you are looking around in the image!.

> **Note**: If the image looks like it is stretched,, try holding the phone vertically or tilt it to see if the image is there.

If you click on the Full Screen icon in the bottom right of the image, the image will be displayed using the entire screen.

If you click the cardboard icon, it will start the view in Cardboard mode for stereo VR mode.

Next step, Let's try some video!