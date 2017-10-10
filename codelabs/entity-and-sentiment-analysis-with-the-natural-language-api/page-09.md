# 9. Analyzing syntax and parts of speech

Looking at the Natural Language API's third method - text annotation - we'll dive deeper into the the linguistic details of our text. annotateText is an advanced method that provides a full set of details on the semantic and syntactic elements of the text. For each word in the text, the API will tell us the word's part of speech (noun, verb, adjective, etc.) and how it relates to other words in the sentence (Is it the root verb? A modifier?).

Let's try it out with a simple sentence. Our JSON request will be similar to the ones above, with the addition of a features key. This will tell the API that we'd like to perform syntax annotation. Replace your request.json with the following:

### request.json

```javascript
 {
  "document":{
    "type":"PLAIN_TEXT",
    "content":"Joanne Rowling is a British novelist, screenwriter and film producer."
  },
  "features":{
    "extractSyntax":true
  }
}
```

Then call the API's `annotateText` method:

```
curl "https://language.googleapis.com/v1/documents:annotateText?key=${API_KEY}" \
  -s -X POST -H "Content-Type: application/json" --data-binary @request.json
```

The response should return an object like the one below for each token in the sentence:

```javascript
{
      "text": {
        "content": "Joanne",
        "beginOffset": -1
      },
      "partOfSpeech": {
        "tag": "NOUN"
      },
      "dependencyEdge": {
        "headTokenIndex": 1,
        "label": "NN"
      },
      "lemma": "Joanne"
}
```

Let's break down the response. `partOfSpeech` tells us that "Joanne" is a noun. `dependencyEdge` includes data that you can use to create a [dependency parse tree](https://en.wikipedia.org/wiki/Parse_tree#Dependency-based_parse_trees) of the text. Essentially, this is a diagram showing how words in a sentence relate to each other. A dependency parse tree for the sentence above would look like this:

![Dependency Parse Tree Imaeg](https://codelabs.developers.google.com/codelabs/cloud-nl-intro/img/1fb62ed60618e914.png)

> **Note:** You can create your own dependency parse trees in the browser with the Natural Language demo available here: [https://cloud.google.com/natural-language/](https://cloud.google.com/natural-language/).

The `headTokenIndex` in our response above is the index of the token that has an arc pointing at "Joanne". We can think of each token in the sentence as a word in an array, and the `headTokenIndex` of 1 for "Joanne" refers to the word "Rowling," which it is connected to in the tree. The label `NN` (short for noun compound modifier) describes the word's role in the sentence. "Joanne" modifies "Rowling," the subject of the sentence. `lemma` is the canonical form of the word. For example, the words _run_, _runs_, _ran_, and _running_ all have a lemma of _run_. The lemma value is useful for tracking occurrences of a word in a large piece of text over time.