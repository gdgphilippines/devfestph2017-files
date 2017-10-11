# 2. Getting set up

### Get the Code

There are two ways to grab the source for this codelab: either download a ZIP file containing the code, or clone it from GitHub.

#### ZIP Download

Click the following button to download all the code for this codelab:

### [**`Download Source Code`**](https://github.com/googlecodelabs/tensorflow-style-transfer-android/archive/codelab-start.zip)

Unpack the downloaded zip file. This will unpack a root folder (`tensorflow-style-transfer-android-codelab-start`), which contains the base app we'll work on in this codelab, including all of the app resources.

#### Check out from GitHub

Check the code out from GitHub:

```git clone https://github.com/googlecodelabs/tensorflow-style-transfer-android```

This will create a directory containing everything you need. If you change into it you can use `git checkout codelab-start` and `git checkout codelab-finish` to switch between the start & end of the lab, respectively.

### Load the code in Android Studio

> **Important**:
> * If you are doing this codelab at Google I/O 2017, you need to do this codelab in **Android Studio 3.0 Canary** - the one with the yellow/gold logo.

Open Android Studio and select **Import Project**. In the file dialog you will need to navigate to **`android`** directory within the directory you downloaded or checked out in the previous step. For example, if you checked out the code into your home directory, you'll want to open `$HOME/tensorflow-style-transfer-android/android`.

If prompted, you should **accept** the suggestion to use the Gradle wrapper and **decline** to use Instant Run.

> **Important**:
> * You need to import/open the **android** directory, **not** the **tensorflow-style-transfer-android** directory


Once Android Studio has imported the project, use the file browser to open the **StylizeActivity** class. This is where we'll work - if you can load the file OK then let's move on to the next section.

> **Note:** The code in this section has been forked from the main TensorFlow repository so that you don't have to compile & check out the whole repository. If you want to build from the latest source you can [check it out from GitHub](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/examples/android/README.md#user-content-building-the-demo-from-source).
