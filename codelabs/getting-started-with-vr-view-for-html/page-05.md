# 5. Capture a panoramic image

While the web page is nice, let's enrich the experience by adding a 360 degree view of I/O. Grab the phone and start the Cardboard Camera app. This app will take a pretty good panoramic picture with a stereoscopic effect.

> **Note**: If you don't have the Cardboard Camera App, or just pressed for time, Don't worry! There is a sample image in the vr view source code. You can find it in `vr_view_101/website/vrview/examples/gallery/taj-mahal.jpg.`
>
> If you use this sample image, make sure to change `is_stereo` parameter to **_false_**.

Click on the camera button and follow the prompts to take a picture. It will take a few seconds to process the image and save it.

### Copy the image to the computer

Once it is saved, we'll copy it from the phone to the computer. The easiest way to do this is reconnect your phone to the computer as a media device. Then copy the Cardboard Camera image which is found in **`DCIM/CardboardCamera.`** It will have a name something like `IMG_20160512_105228.vr.jpg`.

### Supported image and video formats

Whew! Now we have our image, but unfortunately it is not quite in the format we need. VR View has specific image configuration requirements:

*   VR view images can be stored as png, jpeg, or gif. We recommend you use jpeg for improved compression.
*   For maximum compatibility and performance, image dimensions should be powers of two (e.g. 2048 or 4096).
*   Mono images should be 2:1 aspect ratio (e.g. 4096 x 2048).
*   Stereo images should be 1:1 aspect ratio (e.g. 4096 x 4096).
*   The images need to be [equirectangular](https://en.wikipedia.org/wiki/Equirectangular_projection) images. If you have images in other formats such as a [cubemap](https://en.wikipedia.org/wiki/Cube_mapping), they need to be converted to equirectangular in order to be displayed in VR view.

### Format the image

Whew! Now we have our image, but unfortunately it is not quite in the format we need.

If we look at the properties of our image (using Finder/Get Info) it is 10515x1765\. To change it, we'll use a converter page to reformat it.

Open [https://storage.googleapis.com/cardboard-camera-converter/index.html](https://storage.googleapis.com/cardboard-camera-converter/index.html)

Select the image copied or drop it on the page. Then download the converted image and copy it to **`website/images`** and name it **`converted.jpg`**`.`