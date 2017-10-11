# 4. (Re)training the network

### Configure your MobileNet

The retrain script can retrain either [Inception V3 model](https://github.com/tensorflow/models/tree/master/slim#pre-trained-models) or a [MobileNet](https://research.googleblog.com/2017/06/mobilenets-open-source-models-for.html). In this exercise, we will use a MobileNet. The principal difference is that Inception V3 is optimized for accuracy, while the MobileNets are optimized to be small and efficient, at the cost of some accuracy.

Inception V3 has a first-choice accuracy of 78% on ImageNet, but is the model is 85MB, and requires many times more processing than even the largest MobileNet configuration, which achieves 70.5% accuracy, with just a 19MB download.

Pick the following configuration options:

*   Input image resolution: 128,160,192, or 224px. Unsurprisingly, feeding in a higher resolution image takes more processing time, but results in better classification accuracy. We recommend 224 as an initial setting.
*   The relative size of the model as a fraction of the largest MobileNet: 1.0, 0.75, 0.50, or 0.25. We recommend 0.5 as an initial setting. The smaller models run significantly faster, at a cost of accuracy.

With the recommended settings, it typically takes only a couple of minutes to retrain on a laptop. You will pass the settings inside Linux shell variables. Set those shell variables as follows:

```
IMAGE_SIZE=224
ARCHITECTURE="mobilenet_0.50_${IMAGE_SIZE}"
```

The graph below shows the first-choice-accuracies of these configurations (y-axis), vs the number of calculations required (x-axis), and the size of the model (circle area).

16 points are shown for mobilenet. For each of the 4 model sizes (circle area in the figure) there is one point for each image resolution setting. The 128px image size models are represented by the lower-left point in each set, while the 224px models are in the upper right.

Other notable architectures are also included for reference. "GoogleNet" in this figure is ["Inception V1" in this table](https://github.com/tensorflow/models/tree/master/slim#pre-trained-models). An extended version of this figure is available in [slides 84-89 here](http://cs231n.stanford.edu/slides/2017/cs231n_2017_lecture9.pdf).

![Bubble Plot](https://codelabs.developers.google.com/codelabs/tensorflow-for-poets/img/70170cbb89d318b1.png)

### Start TensorBoard

Before starting the training, launch `tensorboard` in the background. TensorBoard is a monitoring and inspection tool included with tensorflow. You will use it to monitor the training progress.

`tensorboard --logdir tf_files/training_summaries &`

>This command will fail with the following error if you already have a tensorboard process running:
>
>`ERROR:tensorflow:TensorBoard attempted to bind to port 6006, but it was already in use`
>
>You can kill all existing TensorBoard instances with:
>
>`pkill -f "tensorboard"`

### Investigate the retraining script

The retrain script is part of the [tensorflow repo](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/examples/image_retraining/retrain.py), but it is not installed as part of the pip package. So for simplicity I've included it in the codelab repository. You can run the script using the python command. Take a minute to skim its "help".

`python -m scripts.retrain -h`

### Run the training

As noted in the introduction, Imagenet models are networks with millions of parameters that can differentiate a large number of classes. We're only training the final layer of that network, so training will end in a reasonable amount of time.

Start your retraining with one big command (note the `--summaries_dir` option, sending training progress reports to the directory that tensorboard is monitoring) :

```
python -m scripts.retrain \
  --bottleneck_dir=tf_files/bottlenecks \
  --how_many_training_steps=500 \
  --model_dir=tf_files/models/ \
  --summaries_dir=tf_files/training_summaries/"${ARCHITECTURE}" \
  --output_graph=tf_files/retrained_graph.pb \
  --output_labels=tf_files/retrained_labels.txt \
  --architecture="${ARCHITECTURE}" \
  --image_dir=tf_files/flower_photos
```

This script downloads the pre-trained model, adds a new final layer, and trains that layer on the flower photos you've downloaded.

>If you are using Docker and the above command fails reporting:
>
>`ERRO[XXXX] error getting events from daemon: EOF`
>
>You have likely encountered [this bug](https://github.com/moby/moby/issues/31220). Increase your Docker cpu allocation to 4 or more, on OSX you can set this by selecting "Preferences..." from the Docker menu, the setting is on the "advanced" tab.

ImageNet does not include any of these flower species we're training on here. However, the kinds of information that make it possible for ImageNet to differentiate among 1,000 classes are also useful for distinguishing other objects. By using this pre-trained network, we are using that information as input to the final classification layer that distinguishes our flower classes.

#### Optional: I'm NOT in a hurry!

The first retraining command iterates only 500 times. You can **very likely get improved results (i.e. higher accuracy) by training for longer**. To get this improvement, remove the parameter `--how_many_training_steps` to use the default 4,000 iterations.

<pre>python -m scripts.retrain \
  --bottleneck_dir=tf_files/bottlenecks \
  --model_dir=tf_files/models/"${ARCHITECTURE}" \
  --summaries_dir=tf_files/training_summaries/"${ARCHITECTURE}" \
  --output_graph=tf_files/retrained_graph.pb \
  --output_labels=tf_files/retrained_labels.txt \
  --architecture="${ARCHITECTURE}" \
  --image_dir=tf_files/flower_photos</pre>

### More about Bottlenecks

_This section and the next provide background on how this retraining process works._

The first phase analyzes all the images on disk and calculates the bottleneck values for each of them. What's a bottleneck?

![Bottleneck Image 1](https://codelabs.developers.google.com/codelabs/tensorflow-for-poets/img/1395d9d6ff9d3e43.png)

These ImageNet models are made up of many layers stacked on top of each other, a simplified picture of Inception V3 from TensorBoard, is shown above (all the details are available [in this paper](http://www.cs.unc.edu/~wliu/papers/GoogLeNet.pdf), with a complete picture on page 6). These layers are pre-trained and are already very valuable at finding and summarizing information that will help classify most images. For this codelab, you are training only the last layer (`final_training_ops` in the figure below). While all the previous layers retain their already-trained state.

![Inception V3 Image 1](https://codelabs.developers.google.com/codelabs/tensorflow-for-poets/img/84a6154ed64fd0fb.png)

In the above figure, the node labeled "softmax", on the left side, is the output layer of the original model. While all the nodes to the right of the "softmax" were added by the retraining script.

>The above figure is a screenshot from tensorboard. [You can open TensorBoard in your browser](http://0.0.0.0:6006/), to get a better look at it. You will find it in the "Graphs" tab.
>
>Note that this will only work after the retrain script finished generating the "bottleneck" files.

A **bottleneck** is an informal term we often use for the layer just before the final output layer that actually does the classification. "Bottelneck" is not used to imply that the layer is slowing down the network. We use the term bottleneck because near the output, the representation is much more compact than in the main body of the network.

Every image is reused multiple times during training. Calculating the layers behind the bottleneck for each image takes a significant amount of time. Since these lower layers of the network are not being modified their outputs can be cached and reused.

So the script is running the constant part of the network, everything below the node labeled `Bottlene...` above, and caching the results.

The command you ran saves these files to the `bottlenecks/` directory. If you rerun the script, they'll be reused, so you don't have to wait for this part again.

### More about Training

Once the script finishes generating all the bottleneck files, the actual training of the final layer of the network begins.

The training operates efficiently by feeding the cached value for each image into the Bottleneck layer. The true label for each image is also fed into the node labeled `GroundTruth.` Just these two inputs are enough to calculate the classification probabilities, training updates, and the various performance metrics.

As it trains you'll see a series of step outputs, each one showing training accuracy, validation accuracy, and the cross entropy:

*   The **training accuracy** shows the percentage of the images used in the current training batch that were labeled with the correct class.
*   **Validation accuracy**: The validation accuracy is the precision (percentage of correctly-labelled images) on a randomly-selected group of images from a different set.
*   **Cross entropy** is a loss function that gives a glimpse into how well the learning process is progressing (lower numbers are better here).

The figures below show an example of the progress of the model's accuracy and cross entropy as it trains. If your model has finished generating the bottleneck files you can check your model's progress by [opening TensorBoard](http://0.0.0.0:6006/), and clicking on the figure's name to show them. TensorBoard may print out warnings to your command line. These can safely be ignored.

![accuracy_1](https://codelabs.developers.google.com/codelabs/tensorflow-for-poets/img/bc758910e1c6eee7.png)

![cross_entropy_1](https://codelabs.developers.google.com/codelabs/tensorflow-for-poets/img/10b16d75eff9b4da.png)

![train_validation](https://codelabs.developers.google.com/codelabs/tensorflow-for-poets/img/ee5d91da57a84831.png)

A true measure of the performance of the network is to measure its performance on a data set that is not in the training data. This performance is measured using the validation accuracy. If the training accuracy is high but the validation accuracy remains low, that means the network is overfitting, and the network is memorizing particular features in the training images that don't help it classify images more generally.

The training's objective is to make the cross entropy as small as possible, so you can tell if the learning is working by keeping an eye on whether the loss keeps trending downwards, ignoring the short-term noise.

By default, this script runs 4,000 training steps. Each step chooses 10 images at random from the training set, finds their bottlenecks from the cache, and feeds them into the final layer to get predictions. Those predictions are then compared against the actual labels to update the final layer's weights through a backpropagation process.

As the process continues, you should see the reported accuracy improve. After all the training steps are complete, the script runs a final test accuracy evaluation on a set of images that are kept separate from the training and validation pictures. This test evaluation provides the best estimate of how the trained model will perform on the classification task.

You should see an accuracy value of between 85% and 99%, though the exact value will vary from run to run since there's randomness in the training process. (If you are only training on two classes, you should expect higher accuracy.) This number value indicates the percentage of the images in the test set that are given the correct label after the model is fully trained.