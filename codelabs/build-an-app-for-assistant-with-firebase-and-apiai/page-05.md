# 5. Create and configure an API.AI project

### Create a new API.AI project

1.  Log into [the API.AI console](https://console.api.ai).
2.  Follow the introduction tutorial for creating your first agent or [click here](https://console.api.ai/api-client/#/newAgent) to go straight into the create agent form.
3.  Fill in with name and description. We recommend:
  *   Name: firebase-animal-guesser
  *   Description: A guessing-game Assistant app hosted entirely in Firebase!

4.  Under "Google Project", select the Firebase project you created in the previous step.
5.  Click **SAVE**

![Create Google Project 1](https://codelabs.developers.google.com/codelabs/assistant-codelab/img/cbd673c54dc0c0d6.png)

### Import some initial API.AI intent(s)

1.  Click ⚙ icon in the left navigation pane to go to agent settings.

  ![API.AI Image 1](https://codelabs.developers.google.com/codelabs/assistant-codelab/img/c33db9a028ca9088.png)

2.  Open the **Export and Import** tab.
3.  Click **RESTORE FROM ZIP**

  ![API.AI Image 1](https://codelabs.developers.google.com/codelabs/assistant-codelab/img/6c0c62551947df72.png)

4.  From the GitHub clone, select **`Assistant.zip`** in the `solution` folder to import the initial "Intents". These intents describe how the conversation flows. You may want to browse them to get a sense of how the game works.
5.  Click **DONE**.

### Update the fulfillment webhook

1.  In the left navigation pane click on Fulfillment.
2.  The Webhook should already be enabled; if it isn't, toggle the ENABLED switch.
3.  Replace or fill in the URL field with this URL:
    https://us-central1-assist-e48e7.cloudfunctions.net/assistantcodelab
4.  Click **SAVE**

### Perform a test conversation

Now we're ready to do a test conversation. We can do this straight from the API.AI console. In the "Try it now" box on the right, type "begin" and hit enter. You should see a response from the welcome intent asking you if you want to play. Go ahead and type "yes" and try out the game! Answer the questions with a simple "yes" or "no", and if it gives up guessing, answer its questions about the animal you were thinking of.

Note that the fulfillment endpoint for the game is being served by an existing backend. In the next step, you'll create your own backend.