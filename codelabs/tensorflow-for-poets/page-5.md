# 5. Using the Retrained Model

The retraining script writes data to the following two files:

*   `tf_files/retrained_graph.pb`, which contains a version of the selected network with a final layer retrained on your categories.
*   `tf_files/retrained_labels.txt`, which is a text file containing labels.

### Classifying an image

The codelab repo also contains a copy of tensorflow's [label_image.py](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/examples/image_retraining/label_image.py) example, which you can use to test your network. Take a minute to read the help for this script:

`python -m  scripts.label_image -h`

As you can see, this Python program takes quite a few arguments. The defaults are all set for this project, but if you used a MobileNet architecture with a different image size you will need to set the `--input_size` argument using the variable you created earlier: --input_size=${IMAGE_SIZE}.

Now, let's run the script on this image of a daisy:

![Daisy Image 1](https://codelabs.developers.google.com/codelabs/tensorflow-for-poets/img/3021186b83bc90c2.png)

`flower_photos/daisy/21652746_cc379e0eea_m.jpg`

Image CC-BY by Retinafunk

```
python -m scripts.label_image \
    --graph=tf_files/retrained_graph.pb  \
    --image=tf_files/flower_photos/daisy/21652746_cc379e0eea_m.jpg
```

Each execution will print a list of flower labels, in most cases with the correct flower on top (though each retrained model may be slightly different).

You might get results like this for a daisy photo:

```
daisy (score = 0.99071)
sunflowers (score = 0.00595)
dandelion (score = 0.00252)
roses (score = 0.00049)
tulips (score = 0.00032)
```

This indicates a high confidence (~99%) that the image is a daisy, and low confidence for any other label.

You can use `label_image.py` to classify any image file you choose, either from your downloaded collection, or new ones. You just have to change the `--image` file name argument to the script.

![Rose Picture 1](https://codelabs.developers.google.com/codelabs/tensorflow-for-poets/img/28cf1a230dc7c51f.png)

`flower_photos/roses/2414954629_3708a1a04d.jpg`

Image CC-BY by Lori Branham

```
python -m scripts.label_image \
    --graph=tf_files/retrained_graph.pb  \
    --image=tf_files/flower_photos/roses/2414954629_3708a1a04d.jpg
```