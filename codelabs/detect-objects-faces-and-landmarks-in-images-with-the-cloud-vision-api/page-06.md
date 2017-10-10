# 6. Upload an Image to a Cloud Storage bucket

### Creating a Cloud Storage bucket

There are two ways to send an image to the Vision API for image detection: by sending the API a base64 encoded image string, or passing it the URL of a file stored in Google Cloud Storage. We'll be using a Cloud Storage URL. The first step to do that is to create a Google Cloud Storage bucket to store our images.

Navigate to Storage in the Cloud console for your project:

![Cloud Console Image 1](https://codelabs.developers.google.com/codelabs/cloud-vision-intro/img/a2691b109903f87.png)

Then click **Create bucket**. Give your bucket a unique name (such as your Project ID) and click **Create**.

![Bucket Image 1](https://codelabs.developers.google.com/codelabs/cloud-vision-intro/img/e1bb21307b891c17.png)

## **Upload an image to your bucket**

Right click on the following image of donuts, then click **Save image as** and save it to your Downloads folder as **donuts.jpeg**.

![Bucket Image 2](https://codelabs.developers.google.com/codelabs/cloud-vision-intro/img/b05d9534e547f5ee.png)

Navigate to the bucket you just created in the storage browser and click **Upload files**. Then select **donuts.jpeg**.

![Bucket Image 3](https://codelabs.developers.google.com/codelabs/cloud-vision-intro/img/fede3cfcc0075d67.png)

You should see the file in your bucket:

![Bucket Image 4](https://codelabs.developers.google.com/codelabs/cloud-vision-intro/img/9fe04ff2faeeb572.png)

Now that you have the file in your bucket, you're ready to create a Vision API request, passing it the URL of this donuts picture.