# 6. Make an Entity Analysis Request

The first Natural Language API method we'll use is `analyzeEntities`. With this method, the API can extract entities (like people, places, and events) from text. To try it out the API's entity analysis, we'll use the following sentence from a recent news article:

_LONDON — J. K. Rowling always said that the seventh Harry Potter book, "Harry Potter and the Deathly Hallows," would be the last in the series, and so far she has kept to her word._

You can build your request to the Natural Language API in a `request.json` file. First create this file in Cloud Shell:

```
touch request.json
```

Open it using your preferred command line editor (`nano`, `vim`, `emacs`). Add the following to your `request.json` file:

### request.json

```
{
  "document":{
    "type":"PLAIN_TEXT",
    "content":"LONDON — J. K. Rowling always said that the seventh Harry Potter book, ‘Harry Potter and the Deathly Hallows,' would be the last in the series, and so far she has kept to her word."
  }
}
```

In the request, we tell the Natural Language API about the text we'll be sending. Supported type values are `PLAIN_TEXT` or `HTML`. In content, we pass the text to send to the Natural Language API for analysis. The Natural Language API also supports sending files stored in Cloud Storage for text processing. If we wanted to send a file from Cloud Storage, we would replace `content` with `gcsContentUri` and give it a value of our text file's uri in Cloud Storage.