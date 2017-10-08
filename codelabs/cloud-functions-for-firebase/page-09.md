# 9. Images moderation

Users can upload all type of images in the chat, and it is always important to moderate offensive images, especially in public social platforms. In FriendlyChat the images that are being published to the chat are stored into [Google Cloud Storage](https://firebase.google.com/docs/storage/).

With Cloud Functions, you can detect new image uploads using the `functions.storage().onChange` trigger. This will run every time a new file is uploaded or modified in Cloud Storage.

To moderate images we'll go through the following process:

1.  Check if the image is flagged as Adult or Violent using the [Cloud Vision API](https://cloud.google.com/vision/)
2.  If the image has been flagged, download it in the running Functions instance
3.  Blur the image using [ImageMagick](https://www.imagemagick.org)
4.  Upload the blurred image to Cloud Storage

### Enable the Cloud Vision API

Since we'll be using the Google Cloud Vision API in this function, you must enable the API on your firebase project. Follow [this link](https://console.cloud.google.com/apis/api/vision.googleapis.com/overview?project=_), select your Firebase project and enable the API:

![Cloud Vision API Image](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/26681849793bf2e5.png)

Note that you will have to [enable billing](https://console.cloud.google.com/billing?project=_) to enable this API. While most of what you do in this codelab should be within the free tier there is a risk you may be billed depending on your usage. If you do not feel comfortable enabling Billing, feel free to skip this section and move on to New Message Notifications.

### Install dependencies

To moderate the images we'll need a few Node.js packages:

*   The Google Cloud Vision Client Library for Node.js: [@google-cloud/vision](https://www.npmjs.com/package/@google-cloud/vision) to run the image through the Cloud Vision API to detect inappropriate images.
*   The Google Cloud Storage Client Library for Node.js: [@google-cloud/storage](https://www.npmjs.com/package/@google-cloud/storage) to download and upload the images from Cloud Storage.
*   A Node.js library allowing us to run processes: [child-process-promise](https://www.npmjs.com/package/child-process-promise) to run ImageMagick since the ImageMagick command-line tool comes pre-installed on all Functions instances.

To install these three packages into your Cloud Functions app, run the following `npm install --save` command. Make sure that you do this from the `functions` directory.

```
npm install --save @google-cloud/vision @google-cloud/storage child-process-promise
```

This will install the three packages locally and add them as declared dependencies in your `package.js` file.

### Import and configure dependencies

To import the three dependencies that were installed and some Node.js core modules (`path`, `os` and `fs`) that we'll need in this section add the following lines to the top of your `index.js` file:

#### index.js

```javascript
const gcs = require('@google-cloud/storage')();
const vision = require('@google-cloud/vision')();
const spawn = require('child-process-promise').spawn;
const path = require('path');
const os = require('os');
const fs = require('fs');
```

Since your function will run inside a Google Cloud environment, there is no need to configure the Cloud Storage and Cloud Vision libraries: they will be configured automatically to use your project.

### Detecting inappropriate images

You'll be using the `functions.storage().onChange` Cloud Functions trigger which runs your code as soon as a file or folder is created or modified in a Cloud Storage bucket. Add the `blurOffensiveImages` Function into your `index.js` file:

#### index.js

```javascript
// Blurs uploaded images that are flagged as Adult or Violence.
exports.blurOffensiveImages = functions.storage.object().onChange(event => {
  const object = event.data;
  // Exit if this is a deletion or a deploy event.
  if (object.resourceState === 'not_exists') {
    return console.log('This is a deletion event.');
  } else if (!object.name) {
    return console.log('This is a deploy event.');
  }

  const image = {
    source: {imageUri: `gs://${object.bucket}/${object.name}`}
  };

  // Check the image content using the Cloud Vision API.
  return vision.safeSearchDetection(image).then(batchAnnotateImagesResponse => {
    const safeSearchResult = batchAnnotateImagesResponse[0].safeSearchAnnotation;
    if (safeSearchResult.adult === 'LIKELY' ||
        safeSearchResult.adult === 'VERY_LIKELY' ||
        safeSearchResult.violence === 'LIKELY' ||
        safeSearchResult.violence === 'VERY_LIKELY') {
      console.log('The image', object.name, 'has been detected as inappropriate.');
      return blurImage(object.name, object.bucket);
    } else {
      console.log('The image', object.name,'has been detected as OK.');
    }
  });
});
```

In the Function above we only care about newly added images, so we just exit if the action was a delete. The image is ran through the Cloud Vision API to detect if it is flagged as adult or violent. If the image is detected as inappropriate based on these criteria we're blurring the image which is done in the `blurImage` function which we'll see next.

### Blurring the image

Add the following `blurImage` function in your `index.js` file:

#### index.js

```javascript
// Blurs the given image located in the given bucket using ImageMagick.
function blurImage(filePath, bucketName, metadata) {
  const tempLocalFile = path.join(os.tmpdir(), path.basename(filePath));
  const messageId = filePath.split(path.sep)[1];
  const bucket = gcs.bucket(bucketName);

  // Download file from bucket.
  return bucket.file(filePath).download({destination: tempLocalFile})
    .then(() => {
      console.log('Image has been downloaded to', tempLocalFile);
      // Blur the image using ImageMagick.
      return spawn('convert', [tempLocalFile, '-channel', 'RGBA', '-blur', '0x24', tempLocalFile]);
    }).then(() => {
      console.log('Image has been blurred');
      // Uploading the Blurred image back into the bucket.
      return bucket.upload(tempLocalFile, {destination: filePath});
    }).then(() => {
      console.log('Blurred image has been uploaded to', filePath);
      // Deleting the local file to free up disk space.
      fs.unlinkSync(tempLocalFile);
      console.log('Deleted local file.');
      // Indicate that the message has been moderated.
      return admin.database().ref(`/messages/${messageId}`).update({moderated: true});
    }).then(() => {
      console.log('Marked the image as moderated in the database.');
    });
}
```

In the above function the image binary is downloaded from Cloud Storage. Then the image is blurred using ImageMagick's `convert` tool and the blurred version is re-uploaded on the Storage Bucket. Then we delete the file on the Cloud Functions instance to free up some disk space, we do this because the same Cloud Functions instance can get re-used and if files are not cleaned up it could run out of disk. Finally we add a boolean to the chat message indicating the image was moderated, this will trigger a refresh of the message on the client.

### Deploy the Function

The Function will only be active after you've deployed it. On the command line run `firebase deploy --only functions`:

```
firebase deploy --only functions
```

This is the console output you should see:

```
i  deploying functions
i  functions: ensuring necessary APIs are enabled...
✔  functions: all necessary APIs are enabled
i  functions: preparing functions directory for uploading...
i  functions: packaged functions (X.XX KB) for uploading
✔  functions: functions folder uploaded successfully
i  starting release process (may take several minutes)...
i  functions: updating function addWelcomeMessages...
i  functions: creating function blurOffensiveImages...
✔  functions[addWelcomeMessages]: Successful update operation.
✔  functions[blurOffensiveImages]: Successful create operation.
✔  functions: all functions deployed successfully!

✔  Deploy complete!

Project Console: https://console.firebase.google.com/project/friendlychat-1234/overview
```

### Test the function

Once the function has deployed successfully:

1.  Open your app in your browser using the hosting URL (in the form of `https://<project-id>.firebaseapp.com`).
2.  Once signed-in the app upload an image: ![Image Icon](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/43fbd8dd2e559aca.png)
3.  Choose your best offensive image to upload (or you can use this [flesh eating Zombie](https://pixabay.com/zombie-flesh-eater-dead-spooky-949916/)!) and after a few moments you should see your post refresh with a blurred version of the image:
    ![Image Attachment Image](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/6a2d6c52e6ea89b.png)