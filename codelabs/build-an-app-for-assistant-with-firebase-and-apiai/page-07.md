# 7. Teaching the App to Learn

### Learn how to differentiate

If you tried to go all the way through the conversation, you may have noticed that it doesn't actually learn how to figure out the guess if it got it wrong. Since that defeats half the purpose of this Assistant App, let's teach it how to learn.

Search `index.js` for `// TODO codelab-1` and replace it and the line following it with this code:

```javascript
const speech = `
I need to know how to tell a ${new_thing} from a ${g_snap.val().a} using a yes-no question.
The answer must be "yes" for ${new_thing}. What question should I use?
`;

const discrmParameters = {};
discrmParameters[LEARN_DISCRIMINATION_PARAM] = true;
assistant.setContext(LEARN_DISCRIM_CONTEXT, 2, discrmParameters);

const answerParameters = {};
answerParameters[ANSWER_PARAM] = new_thing;
assistant.setContext(ANSWER_CONTEXT, 2, answerParameters);

assistant.ask(speech);
```

#### Explanation

Since we already have the intents in the API.AI project set up properly, this ensures that the Cloud Function is setting up the proper contexts and parameters on those contexts. There are three things you should note in this code:

1.  Following the [Actions on Google best practices](https://developers.google.com/actions/design/principles), we change the App's prompt to the user to ensure that there's a clear expectation for what we want them to say next.
2.  We set two API.AI contexts, `learn-discrimination` and `answer`. The lifespan on these contexts is kept pretty short -- two -- to prevent this intent from firing again after the user gave us their question.
3.  For each of the contexts, we set parameters to carry forward into the next intent: `learn-discrimination` is set to keep us within the "learn differentiation" path and `answer` is set to carry forward the new animal the user wants us to learn about.

### Test the addition

Now our Assistant App should be able to learn new things. Deploy the function again with `firebase deploy` and then run through the conversation again forcing it into the learning scenario:

![Assistant App Image](https://codelabs.developers.google.com/codelabs/assistant-codelab/img/237b8c5858682a0a.png)