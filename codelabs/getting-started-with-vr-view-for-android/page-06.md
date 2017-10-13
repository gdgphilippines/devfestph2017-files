# 6. Add the image to the Welcome fragment

Now that we have a panoramic (and stereo!) image, let's add to our application.

### Add VrPanoramaView to the layout

Now that we have the dependencies added, let's edit the layout to replace the static image with a panoramic image.

Open `app/res/layout/welcome_fragment.xml`

> **Heads up!** There are two views of layout files. The Design view and the Text view. Make sure you select Text view at the bottom of the pane.

Find the **ImageView** element and replace it with the **VrPanoramaView**

```xml
    <com.google.vr.sdk.widgets.pano.VrPanoramaView
        android:id="@+id/pano_view"
        android:layout_weight="5"
        android:layout_height="0dp"
        android:layout_margin="5dip"
        android:layout_width="match_parent"
        android:scrollbars="none"
        android:contentDescription="@string/codelab_img_description"/>

```

### Add code to control the VrPanoramaView

Open the file `app/java/com.google.devrel.vrviewapp/WelcomeFragment.java` in Android Studio.

At the top of the class, add a member variable for the view:

```java
private VrPanoramaView panoWidgetView;
```

Make sure the class is imported by adding the import statement above the class:

```java
import com.google.vr.sdk.widgets.pano.VrPanoramaView;
```

> **Pro Tip**: To resolve unknown classes on a mac, click on the class and press ⌥ +⏎ (The option key and return). On other platforms, press Alt-Enter.

Then, in the `onCreateView()` method, initialize the `panoWidgetView` variable from the inflated view. Replace the contents of `onCreateView()` with:

```java
View v =  inflater.inflate(R.layout.welcome_fragment, container,false);
panoWidgetView = (VrPanoramaView) v.findViewById(R.id.pano_view);
return v;
```

Also add `onPause(),onResume(),` and `onDestroy()` methods to pass them to the VrPanoramaView. You should add them just below the `onCreateView()` method.

```java
@Override
public void onPause() {
   panoWidgetView.pauseRendering();
   super.onPause();
}

@Override
public void onResume() {
   panoWidgetView.resumeRendering();
   super.onResume();
}

@Override
public void onDestroy() {
   // Destroy the widget and free memory.
   panoWidgetView.shutdown();
   super.onDestroy();
}

```

Now we have a place to display the image, let's add the code to load it.