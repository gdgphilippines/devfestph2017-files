# 5. Make the model compressible

### Check the compression baseline

The retrained model is still 84MB in size at this point. That large download size may be a limiting factor for any app that includes it.

Every mobile app distribution system compresses the package before distribution. So test how much the graph can be compressed using the gzip command:

```
gzip -c tf_files/optimized_graph.pb > tf_files/optimized_graph.pb.gz

gzip -l tf_files/optimized_graph.pb.gz
```

```
    compressed        uncompressed  ratio     uncompressed_name
    5028302             5460013     7.9%      tf_files/optimized_graph.pb
```

Not much!

On its own, compression is not a huge help. For me this only shaves 8% off the model size. If you're familiar with how neural networks and compression work this should be unsurprising.

The majority of the space taken up by the graph is by the weights, which are large blocks of floating point numbers. Each weight has a slightly different floating point value, with very little regularity.

But compression works by exploiting regularity in the data, which explains the failure here.

### Example: Quantize an Image

Images can also be thought of as large blocks of numbers. One simple technique for compressing images it to reduce the number of colors. You will do the same thing to your network weights, after I demonstrate the effect on an image.

Below I've used [ImageMagick](https://www.imagemagick.org/script/index.php)'s `convert` utility to reduce an image to 32 colors. This reduces the image size by more than a factor of 5 (png has built in compression), but has degraded the image quality.

**Image 1: 24 bit color: 290KB**

![24 bit color: 290KB](https://codelabs.developers.google.com/codelabs/tensorflow-for-poets-2/img/68b7f947c4e09b3e.png)

**Image 2: 32 colors: 55KB**

![32 colors: 55KB](https://codelabs.developers.google.com/codelabs/tensorflow-for-poets-2/img/7551be7b2cd5e1bb.png)

Image CC-BY, by Fabrizio Sciami

### Quantize the network weights

Applying an almost identical process to your neural network weights has a similar effect. It gives a lot more repetition for the compression algorithm to take advantage of, while reducing the precision by a small amount (typically less than a 1% drop in precision).

It does this without any changes to the structure of the network, it simply quantizes the constants in place.

Now use the `quantize_graph` script to apply these changes:

(This script is from the [TensorFlow repository](https://github.com/tensorflow/tensorflow/blob/r1.1/tensorflow/tools/quantization/quantize_graph.py), but it is not included in the default installation)

```
python -m scripts.quantize_graph \
  --input=tf_files/optimized_graph.pb \
  --output=tf_files/rounded_graph.pb \
  --output_node_names=final_result \
  --mode=weights_rounded
```

Now try compressing this quantized model:

```
gzip -c tf_files/rounded_graph.pb > tf_files/rounded_graph.pb.gz

gzip -l tf_files/rounded_graph.pb.gz
```

```
    compressed        uncompressed    ratio   uncompressed_name
    1633131           5460032         70.1%   tf_files/rounded_graph.pb
```

You should see a significant improvement. I get 70% compression instead of the 8% that gzip provided for the original model.

Now before you continue, verify that the quantization process hasn't had too negative an effect on the model's performance.

First manually compare the two models on an example image.

```
python -m scripts.label_image \
  --image=tf_files/flower_photos/daisy/3475870145_685a19116d.jpg \
  --graph=tf_files/optimized_graph.pb
```

```
python -m scripts.label_image \
  --image=tf_files/flower_photos/daisy/3475870145_685a19116d.jpg \
  --graph=tf_files/rounded_graph.pb
```

For me, on this input image, the output probabilities have each changed by less than one tenth of a percent (absolute).

Next verify the change on a larger slice if the data to see how it affects overall performance.

> Note: If you started with the `end_of_first_codelab` branch, instead of working through [TensorFlow for Poets](https://codelabs.developers.google.com/codelabs/tensorflow-for-poets/index.html), you will not have the full set of photos. The model evaluation below will fail. You should either:
>
> 1.  Skip to the next section.
> 2.  Follow the instructions in the other codelab to download the photos.

First evaluate the performance of the baseline model on the validation set. The last two lines of the output show the average performance. It may take a minute or two to get the results back.

`python -m scripts.evaluate  tf_files/optimized_graph.pb`

For me, `optimized_graph.pb` scores 96.1% accuracy, and 0.258 for cross entropy error (lower is better).

Now compare that with the performance of the model in `rounded_graph.pb`:

`python -m scripts.evaluate  tf_files/rounded_graph.pb`

You should see less than a 1% change in the model accuracy.

These differences are far from statistically significant. The goal is simply to confirm that the model was clearly not broken by this change.