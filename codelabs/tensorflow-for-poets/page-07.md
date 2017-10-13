# 7. Optional Step: Training on Your Own Categories

After you see the script working on the flower example images, you can start looking at teaching the network to recognize different categories.

In theory, all you need to do is run the tool, specifying a particular set of sub-folders. Each sub-folder is named after one of your categories and contains only images from that category.

If you complete this step and pass the root folder of the subdirectories as the argument for the `--image_dir` parameter, the script should train the images that you've provided, just like it did for the flowers.

The classification script uses the folder names as label names, and the images inside each folder should be pictures that correspond to that label, as you can see in the flower archive:

![Flower Photos Directory Image 1](https://codelabs.developers.google.com/codelabs/tensorflow-for-poets/img/9444bbae4d5d9ab1.png)

Collect as many pictures of each label as you can and try it out!

>Yufeng Guo used these techniques to make his [candy sorter demo](https://youtu.be/EnFyneRScQ8?t=4m17s). One shortcut he used was to split the training images out of videos of objects from each category, using [ffmpeg](http://stackoverflow.com/questions/10957412/fastest-way-to-extract-frames-using-ffmpeg).