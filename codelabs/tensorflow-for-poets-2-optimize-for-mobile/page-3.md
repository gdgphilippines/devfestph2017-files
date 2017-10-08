# 3. Test the model

Next, verify that the model is producing sane results before starting to modifying it.

The `scripts/` directory contains a simple command line script, `label_image.py`, to test the network. Now we'll test `label_image.py` on this picture of some daisies:

![Daises Image 1](https://codelabs.developers.google.com/codelabs/tensorflow-for-poets-2/img/68b7f947c4e09b3e.png)

`flower_photos/daisy/3475870145_685a19116d.jpg`

Image CC-BY, by Fabrizio Sciami

Now test the model. If you are using a different architecture you will need to set the "--input_size" flag.

```
python -m scripts.label_image \
  --graph=tf_files/retrained_graph.pb  \
  --image=tf_files/flower_photos/daisy/3475870145_685a19116d.jpg
```

The script will print the probability the model has assigned to each flower type. Something like this:

```
daisy 0.94237
roses 0.0487475
sunflowers 0.00510139
dandelion 0.00343337
tulips 0.00034759
```

This should hopefully produce a sensible top label for your example. You'll be using this command to make sure you're still getting sensible results as you do further processing on the model file to prepare it for use in a mobile app.