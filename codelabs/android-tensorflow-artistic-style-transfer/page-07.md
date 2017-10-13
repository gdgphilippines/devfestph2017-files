# Time for a Quiz!

> **Instructions**: 
> * Please answer **all** of the questions below.
> * Submit your answers in a file named, `answers.txt`, as part of your GitHub repo.
> * List your answers following the sample format below. (Note: No spaces between question number & answer)
>
> **answers.txt** 
> ```
> 1.A
> 2.B
> 3.C
> .
> .
> .
> 10.D
> ```

### Questions
1. What is artistic style transfer?
  * **[A]** It is the ability to create a new image, known as a pastiche, based on two input images: one representing the artistic style and one representing the content.
  * **[B]** It is where lower layers identify simple edges and shapes (components of style), and higher levels that identify more complex content, to generate a pastiche.
  * **[C]** It takes frames from the device's camera and renders them toa view on the main activity.

2. What is the helper function, StylizeActivity.onPreviewSizeChosen(...), used for?
  * **[A]**  The app skeleton uses a custom camera fragment that will call this method once permissions have been granted and the camera is available to use.
  * **[B]**  This keeps the style sliders normalised such that their values sum to 1.0, in line with what our network is expecting.
  * **[C]**  Provides a debug overlay when you press the volume up or down buttons on the device, including output from TensorFlow, performance metrics and the original, unstyled, image.

3. What is the helper function, StylizeActivity.setStyle(...), used for?
  * **[A]** The app skeleton uses a custom camera fragment that will call this method once permissions have been granted and the camera is available to use.
  * **[B]** This keeps the style sliders normalised such that their values sum to 1.0, in line with what our network is expecting.
  * **[C]** Provides a debug overlay when you press the volume up or down buttons on the device, including output from TensorFlow, performance metrics and the original, unstyled, image.
  
4. What is the helper function, StylizeActivity.renderDebug(...), used for?
  * **[A]** The app skeleton uses a custom camera fragment that will call this method once permissions have been granted and the camera is available to use.
  * **[B]** This keeps the style sliders normalised such that their values sum to 1.0, in line with what our network is expecting.
  * **[C]** Provides a debug overlay when you press the volume up or down buttons on the device, including output from TensorFlow, performance metrics and the original, unstyled, image.
  
5. What is the helper function, StylizeActivity.stylizeImage(...), used for?
  * **[A]** The app skeleton uses a custom camera fragment that will call this method once permissions have been granted and the camera is available to use.
  * **[B]** This keeps the style sliders normalised such that their values sum to 1.0, in line with what our network is expecting.
  * **[C]** This is where we will do our work. The provided code performs some conversion between arrays of integers (provided by Android's getPixels() method) of the form [0xRRGGBB, ...] to arrays of floats [0.0, 1.0] of the form [r, g, b, r, g, b, ...].
  
6. What is ImageUtils.* for?
  * **[A]** The app skeleton uses a custom camera fragment that will call this method once permissions have been granted and the camera is available to use.
  * **[B]** Provides some helpers for transforming images. The camera provides image data in YUV space (as it is the most widely supported), but the network expects RGB, so we provide helpers to convert the image.
  * **[C]** This is where we will do our work. The provided code performs some conversion between arrays of integers (provided by Android's getPixels() method) of the form [0xRRGGBB, ...] to arrays of floats [0.0, 1.0] of the form [r, g, b, r, g, b, ...].
  
7. To do this codelab at 2017, you need to do this codelab in Android Studio ___?
  * **[A]** 1.0 Canary - the one with the black/green logo.
  * **[B]** 2.0 Canary - the one with the black/silver logo.
  * **[C]** 3.0 Canary - the one with the yellow/gold logo.
  
8. Who introduced a technique that exploits properties of convolutional image classification networks, where lower layers identify simple edges and shapes (components of style), and higher levels identify more complex content, to generate a pastiche?
  * **[A]** Gatys, et al. 2015
  * **[B]** Johnson, et al. 2016
  * **[C]** Dumoulin, et al. 2016
  
9. Whose work intuited that different networks representing different styles would likely by duplicating a lot of information and also proposed a single network trained on multiple styles?
  * **[A]** Gatys, et al. 2015
  * **[B]** Johnson, et al. 2016
  * **[C]** Dumoulin, et al. 2016
  
10. Who proposed a number of improvements including one that makes a trade-off by pre-training networks for each style which results in real-time image generation?
  * **[A]** Gatys, et al. 2015
  * **[B]** Johnson, et al. 2016
  * **[C]** Dumoulin, et al. 2016