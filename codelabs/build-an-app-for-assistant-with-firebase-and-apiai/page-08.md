# 8. Converse and Refine

### Play the game

The only way to tell if our App is working well is, of course, to test it! Play through the game a few times with different outcomes and different responses. Instead of just answering yes, try "sure" or maybe something less definitive like "check." What happens?

Also pay attention to what happens at the end of the game. Does it feel like a natural conclusion to the game? If you were having a conversation, would it feel awkward?

### Training and supporting multiple inputs

One of the benefits of API.AI is that it attempts to match approximate queries to your intents. That is, users aren't always going to say exactly what you expect them to and so the platform tries to learn and match user queries with intents -- even if you didn't specify that exact match! However, it works best when each intent has a wide variety of source material for "user says" queries.

Open up the "Discriminate Yes" intent and expand (if it isn't already) the **User Says** section. You'll see that we only support "Yes". No wonder it wasn't able to respond to something like "check"! Take a moment to fill in a wider variety of ways users could positively answer a question.

![Assistant App Image](https://codelabs.developers.google.com/codelabs/assistant-codelab/img/d077390414438a9.png)

### Completing the conversation naturally

Towards the end of the conversation, you probably noticed that it just ends with a simple "Ok, thanks for the information!" What should the user do next? Are we done?

In a real conversation, we might expect one of two things: either (1) some kind of terminator like "alright, bye now!" or (2) an offer to continue the conversation. Let's build our Assistant App to handle both.

Open up the Function again, `index.js`, and look for `// TODO codelab-2`. Replace it and the line following it with:

```javascript
const speech = `<speak>
OK, thanks for the information! I'll remember to ask "${question}" to see if you're thinking of ${predicate} ${answer}.
<break time="1">
Would you like to play again?
</speak>
`;
assistant.setContext(WELCOME_CONTEXT, 1);
```

Next, go back to the API.AI console, and add a new intent:

![API.AI Console Image 1](https://codelabs.developers.google.com/codelabs/assistant-codelab/img/29bd7733d77dab06.png)

We'll call this intent `game-over` and have it represent a user saying they do not want to play the game. Since there are multiple points in the app where a user can say "no", we'll distinguish this one by setting an input context of `welcome`.

As we did in the Discriminate Yes intent, you should add a wide range of variations on "I don't want to play again." In the interest of time for this codelab, you can just use that single phrase or upload the `game-over.json` file from the solution directory via the **Upload Intent** feature:

![API.AI Console Image 2](https://codelabs.developers.google.com/codelabs/assistant-codelab/img/e2d58c2afcf3b85f.png)

If you created the intent manually, scroll to the bottom of the intent, click on the "**Actions on Google**" header and make sure the "**End Conversation**" box is checked. This tells the Google Assistant that our app has nothing more for the user and closes the mic.