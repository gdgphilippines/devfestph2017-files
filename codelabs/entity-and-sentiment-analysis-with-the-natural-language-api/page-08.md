# 8. Sentiment analysis with the Natural Language API

In addition to extracting entities, the Natural Language API also lets you perform sentiment analysis on a block of text. Our JSON request will include the same parameters as our request above, but this time we'll change the text to include something with a stronger sentiment. Replace your request.json file with the following, and feel free to replace the `content` below with your own text:

### request.json

```javascript
 {
  "document":{
    "type":"PLAIN_TEXT",
    "content":"I love everything about Harry Potter. It's the greatest book ever written."
  }
}
```

Next we'll send the request to the API's `analyzeSentiment` endpoint:

```
curl "https://language.googleapis.com/v1/documents:analyzeSentiment?key=${API_KEY}" \
  -s -X POST -H "Content-Type: application/json" --data-binary @request.json

```

Your response should look like this:

```javascript
{
  "documentSentiment": {
    "polarity": 1,
    "magnitude": 1.8
  },
  "language": "en"
}
```

The sentiment method returns two values, polarity and magnitude. Polarity is a number from -1.0 to 1.0 indicating how positive or negative the statement is. Magnitude is a number ranging from 0 to infinity that represents the weight of sentiment expressed in the statement, regardless of polarity. Longer blocks of text with heavily weighted statements have higher magnitude values. The polarity of our statement above is 100% positive, and the words "love", "greatest", and "ever" contribute to the magnitude value.