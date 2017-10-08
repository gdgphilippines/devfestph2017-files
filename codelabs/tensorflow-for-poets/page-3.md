# 3. Download the training images

Before you start any training, you'll need a set of images to teach the model about the new classes you want to recognize. We've created an archive of creative-commons licensed flower photos to use initially. Download the photos (218 MB) by invoking the following two commands:

```
curl http://download.tensorflow.org/example_images/flower_photos.tgz \
    | tar xz -C tf_files
```

You should now have a copy of the flower photos in your working directory. Confirm the contents of your working directory by issuing the following command:

```
ls tf_files/flower_photos
```

The preceding command should display the following objects:

```
daisy/
dandelion/
roses/
sunflowers/
tulip/
LICENSE.txt
```