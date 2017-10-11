# 10. Explore other Vision API methods

We've looked at the Vision API's label, face, and landmark detection methods, but there are three others we haven't explored. Dive into [the docs](https://cloud.google.com/vision/reference/rest/v1/images/annotate#Feature) to learn about the other three:

*   **Logo detection**: identify common logos and their location in an image.
*   **Safe search detection**: determine whether or not an image contains explicit content. This is useful for any application with user-generated content. You can filter images based on four factors: adult, medical, violent, and spoof content.
*   **Text detection**: run OCR to extract text from images. This method can even identify the language of text present in an image.