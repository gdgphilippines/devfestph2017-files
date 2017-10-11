# 6. Add the image to the webpage

### Edit the HTML

Now that we have a panoramic image, let's add to our webpage.

1.  Start your favorite HTML editor and then open the VrView codelab as an existing project.
2.  Double click on `website/index.html` to open it for editing.
3.  Find the codelab image and replace the img tag with the following iframe.

```html
<iframe
   frameborder="0"
   width="100%"
   scrolling="no"
   allowfullscreen    
   src="vrview/index.html?image=/images/converted.jpg&is_stereo=true">
</iframe>

```

### Save the file and try it out!

Reload the webpage.

VR view is a panoramic view so you can click on the view and drag the image around to see all around the in the image.

Alright! We just added this panoramic view to our web page! Let's take a closer look at what we pasted:

An **Iframe element**. This defines a space on the web page for the VR view code to be loaded and render the image. Inserting the image using an Iframe causes the least interference and complexity with your existing web page. There are several attributes of the Iframe:

*   **frameborder="0"** hides the border around the frame.
*   **width="100%"** makes the frame go full-width.
*   **scrolling="no"** removes the scrollbars from the iframe, this allows moving around your point of view allowing 360 degree viewing.
*   **allowfullscreen** makes it possible for the frame to go into fullscreen mode. (which is the little square in the bottom right of the image).
*   **src="vrview/index.html"** is the relative path to the VR view source code we copied to our site.

#### Control Parameters

If you noticed, in the src attribute, there is a question mark after index.html. This means this page accepts query parameters which control what is displayed:

*   **image** or **video**: {String} URL of image or video to load. (required)

> **Note**: Since we are using an IFRAME to render the image, you must make sure the image or video is available when using [CORS](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing).

*   **is_stereo**: {Boolean} true if content is in stacked stereo format. (optional, default false)
*   **preview**: {String} URL of still preview to load first. (optional, default none)
*   **start_yaw**: {Number} Initial yaw of viewer, in degrees. (optional, default 0)
*   **is_yaw_only**: {Boolean} true if motion is restricted to yaw only. (optional, default false)

Now is a good time to experiment with some of these and see how they affect the VR view experience. Next step, Let's try some video!