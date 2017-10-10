# 10. Multilingual natural language processing

The Natural Language API also supports languages other than English (full list [here](https://cloud.google.com/natural-language/docs/languages)). Let's try the following entity request with a sentence in Japanese:

### request.json

```javascript
{
  "document":{
    "type":"PLAIN_TEXT",
    "content":"日本のグーグルのオフィスは、東京の六本木ヒルズにあります"
  }
}
```

Notice that we didn't tell the API which language our text is, it can automatically detect it. Next, we'll send it to the `analyzeEntities` endpoint:

```
curl "https://language.googleapis.com/v1/documents:analyzeEntities?key=${API_KEY}" \
  -s -X POST -H "Content-Type: application/json" --data-binary @request.json
```

And we get the following response:

```javascript
{
  "entities": [
    {
      "name": "日本",
      "type": "LOCATION",
      "metadata": {
        "wikipedia_url": "http://ja.wikipedia.org/wiki/%E6%97%A5%E6%9C%AC"
      },
      "salience": 0,
      "mentions": [
        {
          "text": {
            "content": "日本",
            "beginOffset": -1
          }
        }
      ]
    },
    {
      "name": "東京",
      "type": "LOCATION",
      "metadata": {
        "wikipedia_url": "http://ja.wikipedia.org/wiki/%E6%9D%B1%E4%BA%AC"
      },
      "salience": 0,
      "mentions": [
        {
          "text": {
            "content": "東京",
            "beginOffset": -1
          }
        }
      ]
    },
    {
      "name": "六本木ヒルズ",
      "type": "LOCATION",
      "metadata": {
        "wikipedia_url": "http://ja.wikipedia.org/wiki/%E5%85%AD%E6%9C%AC%E6%9C%A8%E3%83%92%E3%83%AB%E3%82%BA"
      },
      "salience": 0,
      "mentions": [
        {
          "text": {
            "content": "六本木ヒルズ",
            "beginOffset": -1
          }
        }
      ]
    }
  ],
  "language": "ja"
}
```

The wikipedia URLs even point to the Japanese Wikipedia pages, cool!