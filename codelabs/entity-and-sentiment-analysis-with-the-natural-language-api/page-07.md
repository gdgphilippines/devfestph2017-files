# 7. Call the Natural Language API

You can now pass your request body, along with the API key environment variable you saved earlier, to the Natural Language API with the following `curl` command (all in one single command line):

```
curl "https://language.googleapis.com/v1/documents:analyzeEntities?key=${API_KEY}" \
  -s -X POST -H "Content-Type: application/json" --data-binary @request.json
```

The beginning of your response should look like the following:

```javascript
{
  "entities": [
    {
      "name": "Rowling",
      "type": "PERSON",
      "metadata": {
        "wikipedia_url": "http://en.wikipedia.org/wiki/J._K._Rowling"
      },
      "salience": 0.56932539,
      "mentions": [
        {
          "text": {
            "content": "J. K.",
            "beginOffset": -1
          }
        },
        {
          "text": {
            "content": "Rowling",
            "beginOffset": -1
          }
        }
      ]
    },
    ...
  ]
}
```

In the response, you can see that the API detected four entities from the sentence. For each entity, we get the entity `type`, the associated Wikipedia URL if there is one, the `salience`, and the indexes of where this entity appeared in the text. Salience is a number in the [0,1] range that refers to the centrality of the entity to the text as a whole. In the sentence above, "Rowling" returned the highest salience value since she is the subject of the sentence. The Natural Language API can also recognize the same entity mentioned in different ways. For example, "Rowling," "J.K. Rowling," or even "Joanne Kathleen Rowling" all point to the same Wikipedia entry.​