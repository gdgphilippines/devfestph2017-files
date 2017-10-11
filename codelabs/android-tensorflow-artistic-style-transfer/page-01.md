# 1. Introduction

### What is artistic style transfer?

One of the most exciting developments in deep learning to come out recently is [artistic style transfer](https://arxiv.org/abs/1508.06576), or the ability to create a new image, known as a [pastiche](https://en.wikipedia.org/wiki/Pastiche), based on two input images: one representing the artistic style and one representing the content.

![Image 1](https://codelabs.developers.google.com/codelabs/tensorflow-style-transfer-android/img/c8b30d69a632f9a2.png)

Using this technique, we can generate beautiful new artworks in a range of styles.

![Image 2](https://codelabs.developers.google.com/codelabs/tensorflow-style-transfer-android/img/6c450fc190911a0c.png)

This codelab will walk you through the process of using an artistic style transfer neural network in an Android app in just **9 lines of code**. You can also use the techniques outlined in this codelab to implement any TensorFlow network you have already trained.

> Looking for more? Check out the [Google Research](https://research.googleblog.com/2016/10/supercharging-style-transfer.html) and [Magenta](https://magenta.tensorflow.org/2016/11/01/multistyle-pastiche-generator/) blog posts on this topic.

### What are we going to be building?

In this codelab, you're going to take an existing Android app and add a TensorFlow model to generate stylised images using the device's camera. You'll build the following skills:

*   Using TensorFlow's Android **Java & native libraries** in your app
*   **Importing** a trained TensorFlow model in an Android app
*   Performing **inference** in an Android app
*   Accessing specific **tensors** in a TensorFlow graph

![Image 3](https://codelabs.developers.google.com/codelabs/tensorflow-style-transfer-android/img/8c77dfaea2aa889b.png)

---

### What you'll need

*   An Android device running Lollipop (API 21, v5.0) with a camera supported by the [Camera2 API](https://developer.android.com/reference/android/hardware/camera2/package-summary.html) (introduced in API 21)
*   Android Studio v2.2 or higher
*   Including v23 (Marshmallow) or higher of the SDK build tools

> **Note:** This lab is focused on using an existing TensorFlow model in an Android app. The Android code will largely be provided as-is, but we'll explain the TensorFlow bits, and the TensorFlow-specific Android bits.