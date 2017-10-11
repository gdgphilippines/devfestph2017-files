# 2. Setup

Most of this codelab will be using the terminal. Open it now.

### Install TensorFlow

Before we can begin the tutorial you need to [install tensorflow](https://www.tensorflow.org/install/).

### Use the git repository from the first codelab

This codelab uses files generated during the [TensorFlow for Poets 1](https://codelabs.developers.google.com/codelabs/tensorflow-for-poets/index.html) codelab. If you have not completed that codelab we recommend you go do it now. If you prefer not to, instructions for downloading the missing files are given in the next sub-section.

In TensorFlow for Poets 1, you also cloned the relevant files for this codelab. We will be working in that same git directory, ensure that it is your current working directory, and check the contents, as follows:

```
cd tensorflow-for-poets-2
ls
```

This directory should contain three other subdirectories:

*   The `android/` directory contains all the files necessary to build the a simple Android app that classifies images as it reads them from the camera. The only files missing for the app are those defining the image classification model, which you will create in this tutorial.
*   The `scripts/` directory contains the python scripts you'll be using throughout the tutorial. These include scripts to prepare, test and evaluate the model.
*   The `tf_files/` directory contains the files you should have generated in the first part. At minimum you should have the following files containing the retrained tensorflow program:

`ls tf_files/`

`retrained_graph.pb  retrained_labels.txt`

### Otherwise (if you don't have the files from Part 1)

#### Clone the Git repository

The following command will clone the Git repository containing the files required for this codelab:

`git clone https://github.com/googlecodelabs/tensorflow-for-poets-2`

Now cd into the directory of the clone you just created. That's where you will be working for the rest of this codelab:

`cd tensorflow-for-poets-2`

The repo contains three directories: `android/`, `scripts/, and tf_files/`

#### Checkout the branch with the required files

```
git checkout end_of_first_codelab
ls tf_files/
```