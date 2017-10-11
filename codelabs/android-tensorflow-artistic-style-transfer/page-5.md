# 5. Perform inference using TensorFlow

## Add dependencies to project

To add the inference libraries and their dependencies to our project, we need to add the TensorFlow Android Inference Library and Java API, which is available in [JCenter](https://bintray.com/google/tensorflow/tensorflow-android) or you can build it from the [TensorFlow source](https://github.com/tensorflow/tensorflow/tree/master/tensorflow/contrib/android).

1. Open ![Gradle Icon 1](https://codelabs.developers.google.com/codelabs/tensorflow-style-transfer-android/img/fa9c88ad54abc3bc.png) `build.gradle` in Android Studio.

2. Add the API to the project by adding it to the `dependencies` block within the `android` block (note: this is **not** the `buildscript` block).

    #### [**build.gradle**](https://github.com/googlecodelabs/tensorflow-style-transfer-android/blob/codelab-finish/android/build.gradle)

    ```javascript
    dependencies { 
      compile 'org.tensorflow:tensorflow-android:1.2.0-preview' 
    }
    ```

3. Click the ![Gradle Icon 2](https://codelabs.developers.google.com/codelabs/tensorflow-style-transfer-android/img/4682e6293c1bdb8b.png)**Gradle sync** button to make these changes available in the IDE.

### The TensorFlow Inference Interface

When running TensorFlow code, you would normally need to manage both a computational graph and a session (as covered in the [Getting Started](https://www.tensorflow.org/get_started/get_started#the_computational_graph) docs), however as Android developers will likely want to perform inference over a prebuilt graph, TensorFlow provides a Java interface that manages the graph and session for you: [`TensorFlowInferenceInterface`](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/contrib/android/java/org/tensorflow/contrib/android/TensorFlowInferenceInterface.java).

If you need more control, the TensorFlow Java API provides the familiar [`Session`](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/java/src/main/java/org/tensorflow/Session.java) and [`Graph`](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/java/src/main/java/org/tensorflow/Graph.java) objects you may know from the Python API.

### The Style Transfer Network

We have included the style transfer network described in the last section in the project's ![Directory Icon](https://codelabs.developers.google.com/codelabs/tensorflow-style-transfer-android/img/bb745dc85ae69f6b.png) `assets` directory, so it will be available for you to use already. You can also [download it directly](https://storage.googleapis.com/download.tensorflow.org/models/stylize_v1.zip), or [build it yourself from the Magenta project](https://github.com/tensorflow/magenta/blob/master/magenta/models/image_stylization/README.md).

It may be worth opening the interactive graph viewer so you can see the nodes we will reference shortly (**Hint**: open the `transformer` node by clicking on the + icon that appears once you hover).

[**`Explore the graph interactively`**](https://googlecodelabs.github.io/tensorflow-style-transfer-android/)

### Add the inference code

1. In `StylizeActivity.java`, add the following member fields, near the top of the class (e.g. right before the `NUM_STYLES` declaration)

    ### [StylizeActivity.java](https://github.com/googlecodelabs/tensorflow-style-transfer-android/blob/codelab-finish/android/src/org/tensorflow/demo/StylizeActivity.java)

    ```java
    // Copy these lines below
    private TensorFlowInferenceInterface inferenceInterface;

    private static final String MODEL_FILE = "file:///android_asset/stylize_quantized.pb";

    private static final String INPUT_NODE = "input";
    private static final String STYLE_NODE = "style_num";
    private static final String OUTPUT_NODE = "transformer/expand/conv3/conv/Sigmoid";

    // Do not copy this line, you want to find it and paste before it.
    private static final int NUM_STYLES = 26;
    ```

    > **Note:** Each of these nodes corresponds to a node of the same name in the graph. Try and find these in the interactive graph tool above. Where you see a / (slash character) you will need to **expand** a node to see it's children.

2.  In the same class, find the `onPreviewSizeChosen` method, and construct the `TensorFlowInferenceInterface`. We use this method for initialization as it is called once permissions have been granted to the file system & camera.

    ### [StylizeActivity.java](https://github.com/googlecodelabs/tensorflow-style-transfer-android/blob/codelab-finish/android/src/org/tensorflow/demo/StylizeActivity.java)

    ```java
    @Override
    public void onPreviewSizeChosen(final Size size, final int rotation) {
        // anywhere in here is fine

        inferenceInterface = new TensorFlowInferenceInterface(getAssets(), MODEL_FILE);

        // anywhere at all...
    }
    ```

    > **Important**: If you get a warning about "**Cannot find symbol...**" then you will need to add the import statements into this file. Android Studio can do this for you if you move the cursor onto the red error text, press **Alt-Enter**, and select **Import...**.

3.  Now find the `stylizeImage` method, add the code to pass our camera bitmap and chosen styles to TensorFlow and grab the output from the graph. **This goes in-between the two loops.**

    ### [StylizeActivity.java](https://github.com/googlecodelabs/tensorflow-style-transfer-android/blob/codelab-finish/android/src/org/tensorflow/demo/StylizeActivity.java)

    ```java
    private void stylizeImage(final Bitmap bitmap) {
        // Find the code marked with: TODO: Process the image in TensorFlow here.
        // Then paste the following code in at that location.

        // Start copying here:

        // Copy the input data into TensorFlow.
        inferenceInterface.feed(INPUT_NODE, floatValues, 
        1, bitmap.getWidth(), bitmap.getHeight(), 3);
        inferenceInterface.feed(STYLE_NODE, styleVals, NUM_STYLES);

        // Execute the output node's dependency sub-graph.
        inferenceInterface.run(new String[] {OUTPUT_NODE}, isDebug());

        // Copy the data from TensorFlow back into our array.
        inferenceInterface.fetch(OUTPUT_NODE, floatValues);

        // Don't copy this code, it's already in there.
        for (int i = 0; i < intValues.length; ++i) {
        // ...
    }
     ```

4.  Optional: find `renderDebug` and add the TensorFlow status text to the debug overlay (triggered when you press the volume keys).

    ### [StylizeActivity.java](https://github.com/googlecodelabs/tensorflow-style-transfer-android/blob/codelab-finish/android/src/org/tensorflow/demo/StylizeActivity.java)

    ```java
    private void renderDebug(final Canvas canvas) {
        // ... provided code that does some drawing ...

        // Look for this line, but don't copy it, it's already there.
        final Vector<String> lines = new Vector<>();

        // Add these three lines right here:
        final String[] statLines = inferenceInterface.getStatString().split("\n");
        Collections.addAll(lines, statLines);
        lines.add("");

        // Don't add this line, it's already there
        lines.add("Frame: " + previewWidth + "x" + previewHeight);
        // ... more provided code for rendering the text ...
    }
    ```

    > **Important**: If you get a warning about "**Cannot find symbol...**" then you will need to add the import statements into this file. Android Studio can do this for you if you move the cursor onto the red error text, press **Alt-Enter**, and select **Import...**.

5. In Android Studio, press the ![Play Button Image](https://codelabs.developers.google.com/codelabs/tensorflow-style-transfer-android/img/74540ff4e857014c.png)**Run** button and wait for the project to build.

6. You should now see style transfer happening on your device!

![Android Image 1](https://codelabs.developers.google.com/codelabs/tensorflow-style-transfer-android/img/8c77dfaea2aa889b.png)