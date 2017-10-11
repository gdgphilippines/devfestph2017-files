# 5. Sample dialogs

We use the **sample dialog** process to design the typical conversations the user can have with the persona in your action. It is easiest to start this process by focusing on the ‘happy paths' of the conversation. The happy path assumes there are no technical problems and the user follows the expected path of the conversation. When the happy path is established, we can look at cases where the user goes in an unexpected direction or needs support.

### Invocation greeting

Let's start with how your persona will greet users when they first invoke your action. This is the opportunity for your persona to firmly establish their personality and capabilities in the user's mind. Here's an example, using our Jordan persona:

**User:** _I want to play a science quiz_

**Assistant:** _For a science quiz, you might like talking to Jordan's Science Quiz. Sound good?_

**_User:_ **_Sure, here's Jordan's Science Quiz!_

**Jordan:** _"Hello human, I'm Jordan, I might as well ask you some science questions, I've got nowhere else to be..."_

> **Based on the characteristics you defined for your persona in the previous step, go ahead and write a greeting for your persona in Sample dialog #1 of the template.**

### Questions and answers

After users are introduced to the persona, Jordan will start the quiz by asking the first question, for example:

**Jordan:** "_What gas is used to keep party balloons floating? Hydrogen or Helium?"_

Suggestion buttons - [Hydrogen] [Helium]

Suggestions are the buttons that people can tap to drive the conversation by touch, they also help people understand what they can say.

> **Now write down 3 questions for the science quiz in Sample dialog #2.**

### Correct answers

If the user responds correctly, we should confirm their correct response before moving on to the next step. You should write the correct and incorrect answer prompts as part of the happy path sample dialogs. For example:

**Jordan:** "_What gas is used to keep party balloons floating? Hydrogen or Helium?"_

Suggestion buttons - [Hydrogen] [Helium]

**User:** "_Helium"_

**Jordan:** "_Correct"_

Now, we are trying to model a natural conversation, and in our own language, we avoid repeating ourselves too frequently. So we might want to design differentiation in the dialog for successive correct answers, for example:

Correct #1

**Jordan:** "_Correct"_

Correct #2

**Jordan:** "_Yes, you are a clever human"_

Correct #3

**Jordan:** "_You're on a roll but don't get too excited"_

> **Write down 3 differentiated correct answer prompts for each answer response.**

### Incorrect answers

The action is listening for one of two possible answers, so we should design the dialog for the case in which the user provides the incorrect answer. Again, thinking about dialog strategies that we use in conversation, we can handle this case in several ways. For example, the persona can:

*   Acknowledge that the answer is incorrect.
*   Explain why it's incorrect.
*   Provide a hint for the correct answer.
*   Give the user a second chance.
*   Give the rationale for the correct answer.

For now, Jordan will provide the correct answer and move on to the next question:

**Jordan:** "_What gas is used to keep party balloons floating? Hydrogen or Helium?"_

Suggestion buttons - [Hydrogen] [Helium]

**User:** "_Hydrogen"_

**Jordan:** "_If you're a human you'll need Oxygen."_

Proceed to next question

> **Write down dialog responses for the cases in which the user responds incorrectly to each science question in Sample dialog #2.**

### Completing the quiz

Once the user has answered a few questions, we can think about wrapping up the quiz, using our own conversations as reference, we might say things like:

*   Acknowledge the quiz has ended
*   Provide a summary of results
*   Offer the user to play again

For now, we will simply acknowledge that the quiz has ended. Here's an example from Jordan:

**Jordan:** "_That was the end of the quiz, your intelligence is at an average level for a human."_

**Write down the dialog for completing the science quiz in Sample dialog #3.**

### "Back on track" repair strategies

There are many likely scenarios in which the user falls off the happy path, it's important that we remember that there are real life user conditions that cause this, for example:

*   The user didn't understand
*   The user was interrupted
*   The device lost connection
*   There is too much background noise
*   The user said something valid but it isn't accepted

All of these conditions require repair strategies to get the user back on track and these cases are really important for giving the user confidence in your action.

Let's imagine that Jordan asked a question and the user didn't understand and subsequently doesn't respond. How should Jordan follow-up? Here's an example:

**Jordan:** "_What gas is used to keep party balloons floating? Hydrogen or Helium?"_

Suggestion buttons - [Hydrogen] [Helium]

**User:** "_...?"_

**Jordan:** "_I didn't hear an answer. The choices are: Hydrogen or Helium."_

**User:** "_...?"_

**Jordan:** "_If you're still there, what's your best guess?"_

**User:** "_...?"_

**Jordan:** "_Ok. Let's stop here for now."_

For these cases, it's best practice not to keep prompting the user continuously, here Jordan only prompts 3 times before ending the conversation.

> **Write down the dialog for a scenario in which the user doesn't respond in Sample dialog #4.**