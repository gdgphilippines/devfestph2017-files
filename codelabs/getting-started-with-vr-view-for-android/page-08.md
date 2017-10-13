# 8. Add the video to the gorilla fragment

Capturing panoramic and stereo video is more than we can do on our simple Android phone. For this example, we'll model an encyclopedia page about gorillas. We'll replace a normal image with a panoramic video.

We can add video using the same general approach, just use the video view widget instead of the image one. We'll also add a seekbar control so we can move through the video.

### Add the Video to the layout

Open `res/layout/gorilla_fragment.xml` (using the Text view) and replace the `ImageView` element with the following elements:

```xml
<com.google.vr.sdk.widgets.video.VrVideoView
    android:id="@+id/video_view"
    android:layout_width="match_parent"
    android:scrollbars="none"
    android:layout_height="250dip"/>

<!-- Seeking UI & progress indicator.-->
<SeekBar
    android:id="@+id/seek_bar"
    style="?android:attr/progressBarStyleHorizontal"
    android:layout_height="32dp"
    android:layout_width="fill_parent"/>
<TextView
    android:id="@+id/status_text"
    android:text="Loading Video..."
    android:layout_height="wrap_content"
    android:layout_width="fill_parent"
    android:textSize="12sp"
    android:paddingStart="32dp"
    android:paddingEnd="32dp"/>
```

### Add member variables

Open `app/java/com.google.devrel.vrviewapp/GorillaFragment.java`, and at the top of the class add these member variables:

```java
   /**
     * Preserve the video's state and duration when rotating the phone. This improves 
     * performance when rotating or reloading the video.
     */
    private static final String STATE_IS_PAUSED = "isPaused";
    private static final String STATE_VIDEO_DURATION = "videoDuration";
    private static final String STATE_PROGRESS_TIME = "progressTime";

    /**
     * The video view and its custom UI elements.
     */
    private VrVideoView videoWidgetView;

    /**
     * Seeking UI & progress indicator. The seekBar's progress value represents milliseconds in the
     * video.
     */
    private SeekBar seekBar;
    private TextView statusText;

    /**
     * By default, the video will start playing as soon as it is loaded. 
     */
    private boolean isPaused = false;

```

Remember to add the import statements if they are not already there:

```java
import com.google.vr.sdk.widgets.video.VrVideoView;
import android.widget.SeekBar;
import android.widget.TextView;
```

### Initialize the member variables

Now that we have declared the member variables, we need to initialize them from the inflated view fragment. Find the `onCreateView()` method and replace it with this version which initializes the variables:

```java
@Nullable
@Override
public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
    View view = inflater.inflate(R.layout.gorilla_fragment, container,false);
    seekBar = (SeekBar) view.findViewById(R.id.seek_bar);
    statusText = (TextView) view.findViewById(R.id.status_text);
    videoWidgetView = (VrVideoView) view.findViewById(R.id.video_view);

    // Add the restore state code here.

    // Add the seekbar listener here.

    // Add the VrVideoView listener here

    return view;
}
```

### Initialize the SeekBar Listener

The SeekBar needs to have a listener set to respond to the user changing the seekbar position by moving the video playback to that point.

Add the listener to onCreateView(), just before the return statement.

```java
 // initialize the seekbar listener 
 seekBar.setOnSeekBarChangeListener(new SeekBar.OnSeekBarChangeListener() {
    @Override
    public void onProgressChanged(SeekBar seekBar, int progress, boolean fromUser) {

        // if the user changed the position, seek to the new position.
        if (fromUser) {
            videoWidgetView.seekTo(progress);
            updateStatusText();
        }
    }

    @Override
    public void onStartTrackingTouch(SeekBar seekBar) {
        // ignore for now.
    }

    @Override
    public void onStopTrackingTouch(SeekBar seekBar) {
        // ignore for now.
    }
});
```

> **Note**: The function `updateStatusText()` is not defined yet. It will be added later.

### Initialize the VrVideoView Listener

The VrVideoView needs to have a listener set to respond to the changing state. Specifically:

*   onLoadSuccess - called when the video is successfully loaded.
*   onLoadError - called when there was an error loading the video.
*   onClick - called when the user taps or clicks the video view.
*   onNewFrame - called on each video frame played.
*   onCompletion - called when the video has ended.

Add the listener to onCreateView(), just before the return statement.

```java
// initialize the video listener
videoWidgetView.setEventListener(new VrVideoEventListener() {
    /**
     * Called by video widget on the UI thread when it's done loading the video.
     */
    @Override
    public void onLoadSuccess() {
        Log.i(TAG, "Successfully loaded video " + videoWidgetView.getDuration());
        seekBar.setMax((int) videoWidgetView.getDuration());
        seekBar.setEnabled(true);
        updateStatusText();
   }

   /**
    * Called by video widget on the UI thread on any asynchronous error.
    */
   @Override
   public void onLoadError(String errorMessage) {
       Toast.makeText(
          getActivity(), "Error loading video: " + errorMessage, Toast.LENGTH_LONG)
                      .show();
       Log.e(TAG, "Error loading video: " + errorMessage);
    }

    @Override
    public void onClick() {
        if (isPaused) {
              videoWidgetView.playVideo();
        } else {
            videoWidgetView.pauseVideo();
        }

        isPaused = !isPaused;
        updateStatusText();
    }

    /**
    * Update the UI every frame.
    */
    @Override
    public void onNewFrame() {
        updateStatusText();
        seekBar.setProgress((int) videoWidgetView.getCurrentPosition());
    }

    /**
     * Make the video play in a loop. This method could also be used to move to the next video in
     * a playlist.
     */
    @Override
    public void onCompletion() {
        videoWidgetView.seekTo(0);
    }
});
```

This code references some classes not referenced before, so add the import statements at the top:

```java
import com.google.vr.sdk.widgets.video.VrVideoEventListener;
import android.util.Log;
import android.widget.Toast;
```

This code is almost done, there is a missing member function called in a couple places, `updateStatusText()`. Let's add this at the end of the class.

```java
private void updateStatusText() {
    String status = (isPaused ? "Paused: " : "Playing: ") +
            String.format(Locale.getDefault(), "%.2f", videoWidgetView.getCurrentPosition() / 1000f) +
            " / " +
            videoWidgetView.getDuration() / 1000f +
            " seconds.";
    statusText.setText(status);
}
```

And add another import statement:

```java
import java.util.Locale;
```

### Handle video state saving

When the phone is rotated, the view for the activity is recreated. Since the initial loading of the video is needed to set the seekbar values we want to save this value to use in the event of re-creating the activity. We also want to save the running state of the video so if the phone is rotated, it does not start playing the video if it was paused.

To do this, overload the method: `onSaveInstanceState()`. Add this at the end of the class:

```java
@Override
public void onSaveInstanceState(Bundle savedInstanceState) {
    savedInstanceState.putLong(STATE_PROGRESS_TIME, videoWidgetView.getCurrentPosition());
    savedInstanceState.putLong(STATE_VIDEO_DURATION, videoWidgetView.getDuration());
    savedInstanceState.putBoolean(STATE_IS_PAUSED, isPaused);
    super.onSaveInstanceState(savedInstanceState);
}

```

Then add reading the saved state in the `onCreateView()` method. Add this code right after initializing the member variables, where there is a comment "`// Add the restore state code here.`"

```java
// initialize based on the saved state
if (savedInstanceState != null) {
    long progressTime = savedInstanceState.getLong(STATE_PROGRESS_TIME);
    videoWidgetView.seekTo(progressTime);
    seekBar.setMax((int)savedInstanceState.getLong(STATE_VIDEO_DURATION));
    seekBar.setProgress((int) progressTime);

    isPaused = savedInstanceState.getBoolean(STATE_IS_PAUSED);
    if (isPaused) {
        videoWidgetView.pauseVideo();
    }
} else {
    seekBar.setEnabled(false);
}
```

### Handle lifecycle events

We need to pass the **onPause**, **onResult** and **onDestroy** events into the VrVideoView. Add those methods at the end of the class:

```java
@Override
public void onPause() {
    super.onPause();
    // Prevent the view from rendering continuously when in the background.
    videoWidgetView.pauseRendering();
    // If the video was playing when onPause() is called, the default behavior will be to pause
    // the video and keep it paused when onResume() is called.
    isPaused = true;
}

@Override
public void onResume() {
    super.onResume();
    // Resume the 3D rendering.
    videoWidgetView.resumeRendering();
    // Update the text to account for the paused video in onPause().
    updateStatusText();
}

@Override
public void onDestroy() {
    // Destroy the widget and free memory.
    videoWidgetView.shutdown();
    super.onDestroy();
}

```

### Start the video when visible

Since we are using fragments, there is only one activity for both the WelcomeFragment and the GorillaFragment. We don't want to start loading the video until we switch to the GorillaFragment. To do this, overload `setUserVisibleHint()`in the GorillaFragment. This method will be called when the visibility of the fragment changes.

When the fragment is visible, check to see if the video is loaded, and if not, start loading. Otherwise, just pause the playback since the video is no longer visible.

Add this method to end of the class:

```java
@Override
public void setUserVisibleHint(boolean isVisibleToUser) {
    super.setUserVisibleHint(isVisibleToUser);

    if (isVisibleToUser) {
        try {
            if (videoWidgetView.getDuration() <= 0) {
                videoWidgetView.loadVideoFromAsset("congo_2048.mp4",
                new VrVideoView.Options());
            }
        } catch (Exception e) {
            Toast.makeText(getActivity(), "Error opening video: " + e.getMessage(), Toast.LENGTH_LONG)
                        .show();
        }
    } else {
        isPaused = true;
        if (videoWidgetView != null) {
            videoWidgetView.pauseVideo();
        }
    }
}
```