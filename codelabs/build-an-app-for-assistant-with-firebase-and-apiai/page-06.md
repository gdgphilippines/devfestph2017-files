# 6. Create and deploy an API.AI fulfillment endpoint to Cloud Functions

### Create your working space

Cloud Functions for Firebase gives you some tools to deploy JavaScript that you write on your computer into the Google cloud. Once deployed, it will run in a node.js environment when invoked. The instructions here will set up that space on your computer.

#### Download and install node.js and the Firebase CLI

If you do not already have these installed, download and install [node.js](https://nodejs.org/en/) and the [Firebase CLI](https://firebase.google.com/docs/cli/). Once installed, you should be able to run them from your command line like this to check their versions:

`$ node --version`

`$ firebase --version`

> In general, you should always make sure to keep the Firebase CLI up to date with the following command:
>
> `$ npm install -g firebase-tools`

#### Create and initialize your Cloud Functions workspace

Now, create a folder to hold your project:

<pre>$ mkdir firebase-assistant-codelab
$ cd firebase-assistant-codelab</pre>

To authenticate and get access to your existing project:

`$ firebase login`

You should see a browser window pop up asking for you to allow some permissions:

![Browser Popup Image](https://codelabs.developers.google.com/codelabs/assistant-codelab/img/90a0b915765801a8.png)

After you allow the permissions, you can close that browser window.

Now, initialize your project workspace:

`$ firebase init`

The Firebase CLI can deploy a few different types of things, but you're only going to be using Cloud Functions in this codelab. You can use the arrow keys and spacebar to make sure only "Functions" is selected:

![Firebase Image 1](https://codelabs.developers.google.com/codelabs/assistant-codelab/img/a34cf4e27fa705f7.png)

Answer all the following questions using the default values, hitting enter whenever prompted:

![Firebase Image 2](https://codelabs.developers.google.com/codelabs/assistant-codelab/img/9a942f2478d9b795.png)

Select the Firebase project you created earlier (note that there may be other projects listed here), navigating with the arrow keys if necessary to find it. Press the enter key to select the project. From there, accept any default responses.

You'll now have a "functions" directory ready to hold your code. There is a default `index.js` which is the entry point to your Cloud Functions code. Load it up in a code editor. Overwrite the contents of this file by copying the following code in its place:

```javascript
/**
* Copyright 2017 Google Inc. All Rights Reserved.
*
* Licensed under the Apache License, Version 2.0 (the "License");
* you may not use this file except in compliance with the License.
* You may obtain a copy of the License at
*
*      http://www.apache.org/licenses/LICENSE-2.0
*
* Unless required by applicable law or agreed to in writing, software
* distributed under the License is distributed on an "AS IS" BASIS,
* WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
* See the License for the specific language governing permissions and
* limitations under the License.
*/
'use strict';

process.env.DEBUG = 'actions-on-google:*';

const Assistant = require('actions-on-google').ApiAiAssistant;
const functions = require('firebase-functions');
const admin = require('firebase-admin');
admin.initializeApp(functions.config().firebase);

const know = admin.database().ref('/animal-knowledge');
const graph = know.child('graph');

// API.AI Intent names
const PLAY_INTENT = 'play';
const NO_INTENT = 'discriminate-no';
const YES_INTENT = 'discriminate-yes';
const GIVEUP_INTENT = 'give-up';
const LEARN_THING_INTENT = 'learn-thing';
const LEARN_DISCRIM_INTENT = 'learn-discrimination';

// Contexts
const WELCOME_CONTEXT = 'welcome';
const QUESTION_CONTEXT = 'question';
const GUESS_CONTEXT = 'guess';
const LEARN_THING_CONTEXT = 'learn-thing';
const LEARN_DISCRIM_CONTEXT = 'learn-discrimination';
const ANSWER_CONTEXT = 'answer';

// Context Parameters
const ID_PARAM = 'id';
const BRANCH_PARAM = 'branch';
const LEARN_THING_PARAM = 'learn-thing';
const GUESSABLE_THING_PARAM = 'guessable-thing';
const LEARN_DISCRIMINATION_PARAM = 'learn-discrimination';
const ANSWER_PARAM = 'answer';
const QUESTION_PARAM = 'question';

exports.assistantcodelab = functions.https.onRequest((request, response) => {
   console.log('headers: ' + JSON.stringify(request.headers));
   console.log('body: ' + JSON.stringify(request.body));

   const assistant = new Assistant({request: request, response: response});

   let actionMap = new Map();
   actionMap.set(PLAY_INTENT, play);
   actionMap.set(NO_INTENT, discriminate);
   actionMap.set(YES_INTENT, discriminate);
   actionMap.set(GIVEUP_INTENT, giveUp);
   actionMap.set(LEARN_THING_INTENT, learnThing);
   actionMap.set(LEARN_DISCRIM_INTENT, learnDiscrimination);
   assistant.handleRequest(actionMap);

   function play(assistant) {
       console.log('play');
       const first_ref = know.child('first');
       first_ref.once('value', snap => {
           const first = snap.val();
           console.log(`First: ${first}`);
           graph.child(first).once('value', snap => {
               const speech = `<speak>
Great! Think of an animal, but don't tell me what it is yet. <break time="3"/>
Okay, my first question is: ${snap.val().q}
</speak>`;

               const parameters = {};
               parameters[ID_PARAM] = snap.key;
               assistant.setContext(QUESTION_CONTEXT, 5, parameters);
               assistant.ask(speech);
           });
       });
   }

   function discriminate(assistant) {
       const priorQuestion = assistant.getContextArgument(QUESTION_CONTEXT, ID_PARAM).value;

       const intent = assistant.getIntent();
       let yes_no;
       if (YES_INTENT === intent) {
           yes_no = 'y';
       } else {
           yes_no = 'n';
       }

       console.log(`prior question: ${priorQuestion}`);

       graph.child(priorQuestion).once('value', snap => {
           const next = snap.val()[yes_no];
           graph.child(next).once('value', snap => {
               const node = snap.val();
               if (node.q) {
                   const speech = node.q;

                   const parameters = {};
                   parameters[ID_PARAM] = snap.key;
                   assistant.setContext(QUESTION_CONTEXT, 5, parameters);
                   assistant.ask(speech);
               } else {
                   const guess = node.a;
                   const speech = `Is it a ${guess}?`;

                   const parameters = {};
                   parameters[ID_PARAM] = snap.key;
                   parameters[BRANCH_PARAM] = yes_no;
                   assistant.setContext(GUESS_CONTEXT, 5, parameters);
                   assistant.ask(speech);
               }
           });
       });
   }

   function giveUp(assistant) {
       const priorQuestion = assistant.getContextArgument(QUESTION_CONTEXT, ID_PARAM).value;
       const guess = assistant.getContextArgument(GUESS_CONTEXT, ID_PARAM).value;
       console.log(`Priorq: ${priorQuestion}, guess: ${guess}`);

       const speech = 'I give up!  What are you thinking of?';

       const parameters = {};
       parameters[LEARN_THING_PARAM] = true;
       assistant.setContext(LEARN_THING_CONTEXT, 2, parameters);
       assistant.ask(speech);
   }

   function learnThing(assistant) {
       const priorQuestion = assistant.getContextArgument(QUESTION_CONTEXT, ID_PARAM).value;
       const guess = assistant.getContextArgument(GUESS_CONTEXT, ID_PARAM).value;
       const branch = assistant.getContextArgument(GUESS_CONTEXT, BRANCH_PARAM).value;
       const new_thing = assistant.getArgument(GUESSABLE_THING_PARAM);

       console.log(`Priorq: ${priorQuestion}, guess: ${guess}, branch: ${branch}, thing: ${new_thing}`);

       const q_promise = graph.child(priorQuestion).once('value');
       const g_promise = graph.child(guess).once('value');
       Promise.all([q_promise, g_promise]).then(results => {
           const q_snap = results[0];
           const g_snap = results[1];

           // TODO codelab-1: set the proper contexts to learn the differentiation
           const speech = `I don't know how to tell a ${new_thing} from a ${g_snap.val().a}!`;
           assistant.ask(speech);
       });
   }

   function learnDiscrimination(assistant) {
       const priorQuestion = assistant.getContextArgument(QUESTION_CONTEXT, ID_PARAM).value;
       const guess = assistant.getContextArgument(GUESS_CONTEXT, ID_PARAM).value;
       const branch = assistant.getContextArgument(GUESS_CONTEXT, BRANCH_PARAM).value;
       const answer =  assistant.getContextArgument(ANSWER_CONTEXT, ANSWER_PARAM).value;
       const question = assistant.getArgument(QUESTION_PARAM);

       console.log(`Priorq: ${priorQuestion}, answer: ${answer}, guess: ${guess}, branch: ${branch}, question: ${question}`);

       const a_node = graph.push({
           a: answer
       });

       const q_node = graph.push({
           q: `${question}?`,
           y: a_node.key,
           n: guess
       });

       let predicate = 'a';
       if (['a','e','i','o','u'].indexOf(answer.charAt(0)) != -1) {
           predicate = 'an';
       }

       const update = {};
       update[branch] = q_node.key;
       graph.child(priorQuestion).update(update).then(() => {
           // TODO codelab-2: give the user an option to play again or end the conversation
           const speech = "Ok, thanks for the information!";
           assistant.ask(speech);
       });
   }
});
```

This function responds to HTTPS requests to a dedicated host for your project. You can see that work after you deploy the code.

This function also requires the Google Assistant node.js module, which needs to be installed into the project. Before deploying, you'll need up make sure the proper NPM modules are installed in the functions directory. Use npm to install the dependency in the `functions/` directory:

```
$ cd functions
$ npm install --save actions-on-google
```

#### Deploy the Cloud Functions code

Every time you make changes to your functions, you will need to deploy that to the Google cloud with the following command:

`$ firebase deploy`

**It can take some time for the first deploy to finish - please be patient.**

When the deploy is complete, the CLI will print a message to the console with the URL of the endpoint where your function will respond. Copy that URL from the terminal into a browser to access it.

![Browser Image 2](https://codelabs.developers.google.com/codelabs/assistant-codelab/img/6cda9161d8566312.png)

This function isn't meant to be accessed by a browser, but at least we know it's available for queries. API.AI will use this endpoint to fulfill API requests on behalf of the end user during conversation.

### Configure the API.AI project to point to the Cloud Function endpoint

Now that you have a working endpoint, configure API.AI to use it for fulfillment. Once again, copy the URL of the endpoint from the CLI output into the API.AI project. To find the correct place to paste it:

1.  In the left navigation pane click on **Fulfillment**.
2.  Replace or fill in the URL field with the URL you copied after the completion of `firebase deploy`.
3.  Click **SAVE**

After you commit the change, you can now start a test conversation to see it work. As you test the app, you may see some XML tags come through (for example, `<speak>...</speak>`). We're using something called [Speech Synthesis Markup Language (SSML)](https://developers.google.com/actions/reference/ssml) to add structure and pauses to the responses. The API.AI console doesn't support it, and so shows the raw markup. However, we'll see the effect of using SSML when we get to the Google Assistant integration.

![API.AI Console Image](https://codelabs.developers.google.com/codelabs/assistant-codelab/img/a77a269a26141fde.png)