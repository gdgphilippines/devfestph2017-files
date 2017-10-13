# 3. Load the Android skeleton app

### What's in this app?

This app skeleton contains an Android app that **takes frames from the device's camera and renders them to a view** on the main activity.

#### UI controls

*   The first button, labelled with a number (`256` by default) controls the **size of the image** to display (and eventually run through the style transfer network). Smaller numbers mean smaller images, which will be faster to transform, but will be lower quality. Conversely, bigger images will contain more detail but will take longer to transform.
*   The second button, labelled `save`, will **save the current frame** to your device for you to use later.
*   The thumbnails represent **possible styles** you can use to transform the camera feed. Each image is a slider and you can **combine multiple sliders** that will represent the ratios of each style you wish to apply to your camera frames. These ratios, along with the camera frame, represent the **inputs into the network**.

<img src="https://codelabs.developers.google.com/codelabs/tensorflow-style-transfer-android/img/725a01c7c2722be7.png" alt="Android App Image" width="300" height="500" class="medium"/>

### What is all this extra code?

The app code includes some helpers that are required to interface between native TensorFlow and Android Java. The details of their implementation is not important, but you should understand what they do.

#### `StylizeActivity.onPreviewSizeChosen(...)`

The app skeleton uses a custom camera fragment that will call this method once permissions have been granted and the camera is available to use.

#### `StylizeActivity.setStyle(...)`

This keeps the style sliders normalised such that their values sum to 1.0, in line with what our network is expecting.

#### `StylizeActivity.renderDebug(...)`

Provides a debug overlay when you press the volume up or down buttons on the device, including output from TensorFlow, performance metrics and the original, unstyled, image.

#### `StylizeActivity.stylizeImage(...)`

This is where we will do our work. The provided code performs some conversion between arrays of integers (provided by Android's [`getPixels()`](https://developer.android.com/reference/android/graphics/Bitmap.html#getPixels(int[],%20int,%20int,%20int,%20int,%20int,%20int)) method) of the form `[0xRRGGBB, ...]` to arrays of floats [0.0, 1.0] of the form `[r, g, b, r, g, b, ...]`.

#### `ImageUtils.*`

Provides some helpers for transforming images. The camera provides image data in [YUV space](https://en.wikipedia.org/wiki/YUV) (as it is the most widely supported), but the network expects [RGB](https://en.wikipedia.org/wiki/RGB_color_space), so we provide helpers to convert the image. Most of these are implemented in native C++ for speed; the code is in the [`jni`](https://github.com/tensorflow/tensorflow/tree/master/tensorflow/examples/android/jni) directory but for this lab is provided via the pre-built `libtensorflow_demo.so` binaries in the ![Directory Icon](https://codelabs.developers.google.com/codelabs/tensorflow-style-transfer-android/img/bb745dc85ae69f6b.png)`libs` directory (defined as ![Directory Icon](https://codelabs.developers.google.com/codelabs/tensorflow-style-transfer-android/img/bb745dc85ae69f6b.png)`jniLibs` in Android Studio). If these aren't available, the code will fall back to a Java implementation.