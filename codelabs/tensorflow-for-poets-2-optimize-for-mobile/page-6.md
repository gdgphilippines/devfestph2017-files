# 6. Setup the Android app

![Android Image](https://codelabs.developers.google.com/codelabs/tensorflow-for-poets-2/img/cd053a79396f5805.png)

### Add your model to the project

The demo project is configured to search for the `rounded_graph.pb`, and `retrained_labels.txt` files in the `android/assets` directory. Copy the files you just created into the expected location:

```
cp tf_files/rounded_graph.pb android/assets/graph.pb
cp tf_files/retrained_labels.txt android/assets/labels.txt 
```

### Install AndroidStudio

If you don't have it installed already, go [install AndroidStudio](https://developer.android.com/studio/index.html).

### Open the project with AndroidStudio

Open a project with AndroidStudio by taking the following steps:

1.  Open AndroidStudio. After it loads select "![Folder Icon](https://codelabs.developers.google.com/codelabs/tensorflow-for-poets-2/img/bb745dc85ae69f6b.png) Open an existing Android Studio project" from this popup:

  ![Android Studio Image](https://codelabs.developers.google.com/codelabs/tensorflow-for-poets-2/img/1482ddc7911df61b.png)

2.  In the file selector, choose `tensorflow-for-poets-2/android` from your working directory.

3.  You will get a "Gradle Sync" popup, the first time you open the project, asking about using gradle wrapper. Click "OK".

  ![Gradle Sync Image](https://codelabs.developers.google.com/codelabs/tensorflow-for-poets-2/img/b9f9a03dd27fd1bb.png)

> If you get a Gradle sync error:
>
> ![Gradle Sync Error Image](https://codelabs.developers.google.com/codelabs/tensorflow-for-poets-2/img/186d73f2db74031.png)
>
> It's because gradle couldn't find `android/assets/graph.pb`, or `android/assets/labels.txt`. Verify the locations of those files and re-run the gradle sync by clicking the "Sync Project with Gradle Files" button from the toolbar:
>
> ![Gradle Sync Icons](https://codelabs.developers.google.com/codelabs/tensorflow-for-poets-2/img/774326d4e89c2559.png)

### Change the `output_name` in `ClassifierActivity.java`

The app is currently set up to run the baseline MobileNet. The output node for our model has a different name. Open `ClassifierActivity.java` and update the `OUTPUT_NAME` variable.

[ClassifierActivity.java](https://github.com/googlecodelabs/tensorflow-for-poets-2/blob/master/android/src/org/tensorflow/demo/ClassifierActivity.java)

```java
  private static final String INPUT_NAME = "input";
  private static final String OUTPUT_NAME = "final_result";
```