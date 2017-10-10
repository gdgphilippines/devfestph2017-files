# 8. Call the Vision API's label detection method

Before calling the Vision API, we need to update the access on the image in our bucket to allow the Vision API read access. Run the following command to allow global read access on this image, making sure to replace **my-storage-bucket** with the name of your bucket. This command uses `gsutil`, which lets you access Cloud Storage from the command line and comes installed with your Cloud Shell environment by default.

```
gsutil acl ch -g AllUsers:R gs://my-storage-bucket/donuts.jpeg
```

Now we're ready to call the Vision API with curl:

```
curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json  https://vision.googleapis.com/v1/images:annotate?key=${API_KEY}
```

Your response should look something like the following:

```javascript
{
  "labelAnnotations": [
    {
      "mid": "/m/02wbm",
      "description": "Food",
      "score": 94
    },
    {
      "mid": "/m/0ggjl84",
      "description": "Baked Goods",
      "score": 90
    },
    {
      "mid": "/m/02q08p0",
      "description": "Dish",
      "score": 85
    },
    {
      "mid": "/m/0270h",
      "description": "Dessert",
      "score": 83
    },
    {
      "mid": "/m/0bp3f6m",
      "description": "Fried Food",
      "score": 75
    },
    {
      "mid": "/m/01wydv",
      "description": "Beignet",
      "score": 67
    },
    {
      "mid": "/m/0pqdc",
      "description": "Hors D Oeuvre",
      "score": 54
    }
  ]
}
```

The API was able to identify the specific type of donuts these are (beignets), cool! For each label the Vision API found, it returns a `description` with the name of the item. It also returns a `score`, a number from 0 - 100 indicating how confident it is that the description matches what's in the image. The `mid` value maps to the item's mid in Google's [Knowledge Graph](https://www.google.com/intl/bn/insidesearch/features/search/knowledge.html). You can use the `mid` when calling the [Knowledge Graph API](https://developers.google.com/knowledge-graph/) to get more information on the item.