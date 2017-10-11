# 4. A learned representation of artistic style

## About this network

> **Note:** While it's not critical to understand how this network works in order to use it or import your own, some background is provided here. Feel free to skip this section.

The network we are importing is a result of a number of important developments. The first neural style transfer paper ([Gatys, et al. 2015](http://arxiv.org/abs/1508.06576)) introduced a technique that exploits properties of convolutional image classification networks, where lower layers identify simple edges and shapes (components of style), and higher levels identify more complex content, to generate a pastiche. This technique works on any two images but is slow to execute.

A number of improvements have since been proposed, including one that makes a trade-off by pre-training networks for each style ([Johnson, et al. 2016](https://arxiv.org/abs/1603.08155)), resulting in real-time image generation.

Finally, the network we use in this lab ([Dumoulin, et al. 2016](https://arxiv.org/abs/1610.07629)) intuited that different networks representing different styles would likely be duplicating a lot of information, and proposed a single network trained on multiple styles. An interesting side-effect of this was the ability to combine styles, which we are using here.

For a more technical comparison of these networks, as well as review of others, check out [Cinjon Resnick's review article](https://github.com/tensorflow/magenta/blob/master/magenta/reviews/styletransfer.md).

### Inside the network

The original TensorFlow code that generated this network is available on [Magenta's GitHub page](https://github.com/tensorflow/magenta), specifically the [stylized image transformation model](https://github.com/tensorflow/magenta/blob/master/magenta/models/image_stylization/model.py#L28) ([README](https://github.com/tensorflow/magenta/blob/master/magenta/models/image_stylization/README.md)).

Before using it in an environment with constrained resources, such as a mobile app, this model was exported and transformed to use smaller data types & remove redundant calculations. You can read more about this process in the [Graph Transforms](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/tools/graph_transforms/README.md) doc, and try it out in the [TensorFlow for Poets II: Optimize for Mobile](https://codelabs.developers.google.com/codelabs/tensorflow-for-poets-2/) codelab.

The end result is the [`stylize_quantized.pb`](https://github.com/googlecodelabs/tensorflow-style-transfer-android/blob/master/android/assets/stylize_quantized.pb) file, displayed below, that you will use in the app. The `transformer` node contains most of the graph, click through to the interactive version to expand it.

![model](https://codelabs.developers.google.com/codelabs/tensorflow-style-transfer-android/img/dac7f10dfec75e53.png)

### [**`Explore this graph interactively`**](https://googlecodelabs.github.io/tensorflow-style-transfer-android/)
