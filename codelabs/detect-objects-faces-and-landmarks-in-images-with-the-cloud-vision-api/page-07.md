# 7. Create your Vision API request
In your Cloud Shell environment, create a `request.json` file with the following, making sure to replace **my-bucket-name** with the name of the Cloud Storage bucket you created:

> **Note:** Use one of your preferred command line editors (nano, vim, or emacs)

### request.json

```javascript
{
  "requests": [
      {
        "image": {
          "source": {
              "gcsImageUri": "gs://my-bucket-name/donuts.jpeg"
          } 
        },
        "features": [
          {
            "type": "LABEL_DETECTION",
            "maxResults": 10
          }
        ]
      }
  ]
}
```

> **Note:** Replace **my-bucket-name** with the name of your storage bucket.

The first Cloud Vision API feature we'll explore is label detection. This method will return a list of labels (words) of what's in your image.