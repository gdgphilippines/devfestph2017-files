# 7. Add VR View Video to our web site

Capturing panoramic and stereo video is more than we can do on our simple Android phone. For this example, we'll model an encyclopedia page about gorillas. We'll replace a normal image with a panoramic video.

Adding video uses the same approach, just use the video parameter instead of the image parameter.

Open the file **`website/gorillas.html`**.

Find the div element where the id equals "photo".

Replace that entire div element with the iframe:

```html
<iframe
  frameborder="0"
  width="100%"
  scrolling="no"
  allowfullscreen
  src="vrview/index.html?video=/vrview/examples/video/congo_2048.mp4&is_stereo=true">
</iframe>
```

Save the file and reload the page in the browser to see the updated page.