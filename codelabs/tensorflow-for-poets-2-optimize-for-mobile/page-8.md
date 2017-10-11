# 8. How does it work?

So now that you have the app running, let's look at the TensorFlow specific code.

### TensorFlow-Android AAR

This app uses a pre-compiled Android Archive (AAR) for its TensorFlow dependencies. This AAR is hosted on [jcenter](https://bintray.com/google/tensorflow/tensorflow-android). The code to build the AAR lives in [tensorflow.contrib.android](https://github.com/tensorflow/tensorflow/tree/master/tensorflow/contrib/android).

The following lines in the [build.gradle](https://github.com/googlecodelabs/tensorflow-for-poets-2/blob/master/android/build.gradle) file include the AAR in the project.

[build.gradle](https://github.com/googlecodelabs/tensorflow-for-poets-2/blob/master/android/build.gradle)

```java
repositories {
   jcenter()
}

dependencies {
   compile 'org.tensorflow:tensorflow-android:+'
}
```

### Using the TensorFlow Inference Interface

The code interfacing to the TensorFlow is all contained in [TensorFlowImageClassifier.java](https://github.com/googlecodelabs/tensorflow-for-poets-2/blob/master/android/src/org/tensorflow/demo/TensorFlowImageClassifier.java).

#### Create the Interface

The first block of interest simply creates a `TensorFlowInferenceInterface`, which loads the named `TensorFlow` graph using the `assetManager`.

This is similar to a `tf.Session` (for those familiar with TensorFlow in Python).

[TensorFlowImageClassifier.java](https://github.com/googlecodelabs/tensorflow-for-poets-2/blob/master/android/src/org/tensorflow/demo/TensorFlowImageClassifier.java)

```java
// load the model into a TensorFlowInferenceInterface.
c.inferenceInterface = new TensorFlowInferenceInterface(
    assetManager, modelFilename);
```

#### Inspect the output node

This model can be retrained with different numbers of output classes. To ensure that we build an output array with the right size, we inspect the TensorFlow operations:

[TensorFlowImageClassifier.java](https://github.com/googlecodelabs/tensorflow-for-poets-2/blob/master/android/src/org/tensorflow/demo/TensorFlowImageClassifier.java)

```java
// Get the tensorflow node
final Operation operation = c.inferenceInterface.graphOperation(outputName);

// Inspect its shape
final int numClasses = (int) operation.output(0).shape().size(1);

// Build the output array with the correct size.
c.outputs = new float[numClasses];

```

#### Feed in the input

To run the network, we need to feed in our data. We use the `feed` method for that. To use `feed,` we must pass:

*   the name of the node to feed the data to
*   the data to put in that node
*   the shape of the data

The following lines execute the feed method.

[TensorFlowImageClassifier.java](https://github.com/googlecodelabs/tensorflow-for-poets-2/blob/master/android/src/org/tensorflow/demo/TensorFlowImageClassifier.java)

```java
inferenceInterface.feed(
    inputName,   // The name of the node to feed. 
    floatValues, // The array to feed
    1, inputSize, inputSize, 3 ); // The shape of the array
```

> FAQ: An image is a 3D array (width, height, color), why does the shape here have 4 dimensions?
>
> This model is built to run stacks of images (with identical sizes) in a single execution. The first index is across the image stack. Here we are feeding in a "stack" containing a single image.

#### Execute the calculation

Now that the inputs are in place, we can run the calculation.

Note how this `run` method takes an array of output names because you may want to pull more than one output. It also accepts a boolean flag to control logging.

[TensorFlowImageClassifier.java](https://github.com/googlecodelabs/tensorflow-for-poets-2/blob/master/android/src/org/tensorflow/demo/TensorFlowImageClassifier.java)

```java
inferenceInterface.run(
    outputNames, // Names of all the nodes to calculate.
    logStats);   // Bool, enable stat logging.
```

#### Fetch the output

Now that the output has been calculated we can pull it out of the model into a local variable.
The `outputs` array here is the one we sized by inspecting the output `Operation` earlier.

Call this fetch method once for each output you wish to fetch.

[TensorFlowImageClassifier.java](https://github.com/googlecodelabs/tensorflow-for-poets-2/blob/master/android/src/org/tensorflow/demo/TensorFlowImageClassifier.java)

```java
inferenceInterface.fetch(
    outputName,  // Fetch this output.
    outputs);    // Into the prepared array.
```