# 9. Face and Landmark Detection with the Vision API

Next we'll explore the face and landmark detection methods of the Vision API. The face detection method returns data on faces found in an image, including the emotions of the faces and their location in the image. Landmark detection can identify common (and obscure) landmarks - it returns the name of the landmark, its latitude longitude coordinates, and the location of where the landmark was identified in an image.

### Upload a new image

To use these two new methods, let's upload a new image with faces and landmarks to our Cloud Storage bucket. Right click on the following image, then click **Save image as** and save it to your Downloads folder as **selfie.jpeg**.

![Selfie Sample Image 1](https://codelabs.developers.google.com/codelabs/cloud-vision-intro/img/4bafc91d806a0425.png)

Then upload it to your Cloud Storage bucket the same way you did in the previous step. After uploading the photo, update the access control on the new image with the following `gsutil` command:

```
gsutil acl ch -g AllUsers:R gs://my-storage-bucket/selfie.jpeg
```

### Updating our request

Next, we'll update our `request.json` file to include the URL of the new image, and to use face and landmark detection instead of label detection. Be sure to replace **my-bucket-name** with the name of our Cloud Storage bucket:

#### request.json

```javascript
{
  "requests": [
      {
        "image": {
          "source": {
              "gcsImageUri": "gs://my-bucket-name/selfie.jpeg"
          } 
        },
        "features": [
          {
            "type": "FACE_DETECTION"
          },
          {
            "type": "LANDMARK_DETECTION"
          }
        ]
      }
  ]
}
```

### Calling the Vision API and parsing the response

Now you're ready to call the Vision API using the same curl command you used above:

```
curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json  https://vision.googleapis.com/v1/images:annotate?key=${API_KEY}
```

Let's take a look at the `faceAnnotations` object in our response first. You'll notice the API returns an object for each face found in the image - in this case, three. Here's a clipped version of our response:

```javascript
{
      "faceAnnotations": [
        {
          "boundingPoly": {
            "vertices": [
              {
                "x": 669,
                "y": 324
              },
              ...
            ]
          },
          "fdBoundingPoly": {
            ...
          },
          "landmarks": [
            {
              "type": "LEFT_EYE",
              "position": {
                "x": 692.05646,
                "y": 372.95868,
                "z": -0.00025268539
              }
            },
            ...
          ],
          "rollAngle": 0.21619819,
          "panAngle": -23.027969,
          "tiltAngle": -1.5531756,
          "detectionConfidence": 0.72354823,
          "landmarkingConfidence": 0.20047489,
          "joyLikelihood": "POSSIBLE",
          "sorrowLikelihood": "VERY_UNLIKELY",
          "angerLikelihood": "VERY_UNLIKELY",
          "surpriseLikelihood": "VERY_UNLIKELY",
          "underExposedLikelihood": "VERY_UNLIKELY",
          "blurredLikelihood": "VERY_UNLIKELY",
          "headwearLikelihood": "VERY_LIKELY"
        }
        ...
     }
}
```

The `boundingPoly` gives us the x,y coordinates around the face in the image. `fdBoundingPoly` is a smaller box than `boundingPoly`, encodling on the skin part of the face. `landmarks` is an array of objects for each facial feature (some you may not have even known about!). This tells us the type of landmark, along with the 3D position of that feature (x,y,z coordinates) where the z coordinate is the depth. The remaining values gives us more details on the face, including the likelihood of joy, sorrow, anger, and surprise. The object above is for the person furthest back in the image - you can see he's making kind of a silly face which explains the `joyLikelihood` of `POSSIBLE`.

Next let's look at the `landmarkAnnotations` part of our response:

```javascript
"landmarkAnnotations": [
        {
          "mid": "/m/0c7zy",
          "description": "Petra",
          "score": 0.5403372,
          "boundingPoly": {
            "vertices": [
              {
                "x": 153,
                "y": 64
              },
              ...
            ]
          },
          "locations": [
            {
              "latLng": {
                "latitude": 30.323975,
                "longitude": 35.449361
              }
            }
          ]
```

Here, the Vision API was able to tell that this picture was taken in Petra - this is pretty impressive given the visual clues in this image are minimal. The values in this response should look similar to the `labelAnnotations` response above.

We get the `mid` of the landmark, it's name (`description`), along with a confidence `score`. `boundingPoly` shows the region in the image where the landmark was identified. The `locations` key tells us the latitude longitude coordinates of [this landmark](https://www.google.com/?ion=1&espv=2#q=30.323975%2C%2035.449361).