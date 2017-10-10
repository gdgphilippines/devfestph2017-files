# 3. Create your first Agent

After the login you can create your first agent.

See below:

![API.AI Image 1](https://codelabs.developers.google.com/codelabs/your-first-action-on-google-with-webhook/img/66409e1661e461e8.png)

You will need to:

1.  Click on ‘Create Agent' and give your agent a name.
    In our case, it will be "BitcoinInfo". Please note that the agent name can not contain any spaces between the words.
2.  Give a short description so other users will know what this action is going to do.
3.  Click on ‘Save'.
    It's the button in the top-right corner of the screen.

### Create our first Entity

#### What are entities?

Entities are the values we are trying to capture from the user phrases. Kind of like filling out a form, requesting details from the user. API.AI looks to extract these out, and will do follow up prompts until done.

This is how an entity looks in API.AI

![API.AI Image 2](https://codelabs.developers.google.com/codelabs/your-first-action-on-google-with-webhook/img/96843a161ca83b14.png)

We will create a currency entity.

(!) Actually, API.AI already has this type of entity built out for you. But for the purpose of this codelab we will create it, cool?

#### Create a Bitcoin entity

First step is to click on the ‘Create Entity' button.

![BitCoin Entity Image 1](https://codelabs.developers.google.com/codelabs/your-first-action-on-google-with-webhook/img/6419ab816431e9e2.png)

#### Important aspects to note:

1.  You should ‘help' our machine learning algorithm to train itself by providing synonyms. the synonyms list is kind of like a lookup table of words to other words. For example, we typed few currencies like: Euro, Dollars, Bitcoins etc'. Moreover, we allowed API.AI to use its machine learning to expand this entity by clicking on the checkbox ‘Allow automated expansion'.
2.  In the real world, try to give many examples so it will cover more cases.
3.  Click on ‘Save' when you are done.

### Build The Webhook

We got for you two options here:

1.  If you don't have a server running right now, You can use our demo live webhook: [https://us-central1-bitcoin-info-io17.cloudfunctions.net/bitcoinInfo](https://us-central1-bitcoin-info-io17.cloudfunctions.net/bitcoinInfo)
    by just plug it to your Fulfilment.

2.  If you wish to test the webhook from your own server. You can take the code below and use your own server with NodeJS.
    This is the index.js that contain all the logic for our webhook.

#### [index.js](https://github.com/greenido/bitcoin-info-action/blob/master/index.js)

```javascript
// Copyright 2017, Google, Inc.
// Licensed under the Apache License, Version 2.0 (the 'License');
// you may not use this file except in compliance with the License.
// You may obtain a copy of the License at
//
//    http://www.apache.org/licenses/LICENSE-2.0
//
// Unless required by applicable law or agreed to in writing, software
// distributed under the License is distributed on an 'AS IS' BASIS,
// WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
// See the License for the specific language governing permissions and
// limitations under the License.

'use strict';

process.env.DEBUG = 'actions-on-google:*';

// We need the Assistant client for all the magic here
const Assistant = require('actions-on-google').ApiAiAssistant;
// To make our http request (a bit nicer)
const request = require('request');

// Some variables we will use in this example
const ACTION_PRICE = 'price';
const ACTION_TOTAL = 'total';
const EXT_BITCOIN_API_URL = "https://blockchain.info";
const EXT_PRICE = "/q/24hrprice";
const EXT_TOTAL = "/q/totalbc";

// [START bitcoinInfo]
exports.bitcoinInfo = (req, res) => {
  const assistant = new Assistant({request: req, response: res});
  console.log('bitcoinInfoAction Request headers: ' + JSON.stringify(req.headers));
  console.log('bitcoinInfoAction Request body: ' + JSON.stringify(req.body));

  // Fulfill price action business logic
  function priceHandler (assistant) {
    request(EXT_BITCOIN_API_URL + EXT_PRICE, function(error, response, body) {
      // The fulfillment logic for returning the bitcoin current price
    console.log("priceHandler response: " + JSON.stringify(response) + " Body: " + body + " | Error: " + error);
    const msg = "Right now the price of a bitcoin is " + body + " USD";
    assistant.tell(msg);
    });
  }

  // Fulfill total bitcoin action 
  function totalHandler (assistant) {
    request(EXT_BITCOIN_API_URL + EXT_TOTAL, function(error, response, body) {
    console.log("totalHandler response: " + JSON.stringify(response) + " Body: " + body + " | Error: " + error);
    // The fulfillment logic for returning the amount of bitcoins in the world
    const billionsBitcoins = body / 1000000000;
    const msg = "Right now there are " + billionsBitcoins + " billion bitcoins around the world.";
    assistant.tell(msg);
    });
  }

  // The Entry point to all our actions
  const actionMap = new Map();
  actionMap.set(ACTION_PRICE, priceHandler);
  actionMap.set(ACTION_TOTAL, totalHandler);

  assistant.handleRequest(actionMap);
};
// [END bitcoinInfo]
```

### Create our intents

#### What is an intent?

An Intent is triggered by a series of "user says" phrases. This could be something like "Please tell me what is the bitcoin value in USD" or "What is the block chain size today?".

In most cases, you need to specify 10-12 sentences to train API.AI's machine learning algorithm. Then even if the user doesn't say exactly the words you typed here, API.AI can still understand them!

![API.AI Image 3](https://codelabs.developers.google.com/codelabs/your-first-action-on-google-with-webhook/img/5f8a76b120f219a9.png)

You should create separate intents for different types of actions though.

Don't try to combine all of them together.

In our example, we will create only three intents:

*   **Price intent** - This intent will handle the main actions: fetching the bitcoin price.
*   **Total intent** - This intent will tell user the number of existing bitcoins in the world
*   **Quit intent** - This intent will handle the part when the user wishes to finish the action.

#### Build the "price" intent

After we have our new @currency entity. If you notice the @ before the word - It's not a mistake. This is the way we will refer to our new entity from now. Think of it as a special sign to show us that we are referring to our entity and not just another bitcoin.

It's time to create the intent that will fetch us bitcoin information.

First, click on the ‘Create Intent' button.

![API.AI Image 4](https://codelabs.developers.google.com/codelabs/your-first-action-on-google-with-webhook/img/ba35bfa29ab09f1e.png)

Second, start typing few sentences that you will want to use to get information on bitcoin. For example, "please tell me what is the value of bitcoin today in USD".

Type a few sentences so API.AI could start training its algorithms.

You can see that while you type, API.AI automatically recognizes that the phrase includes one of the entities, so it highlights it.

![API.AI Image 5](https://codelabs.developers.google.com/codelabs/your-first-action-on-google-with-webhook/img/13d4efe2e75fd3f7.png)

Next, we are skipping the ‘events' part, and in the ‘Action' section we need:

1.  Type ‘price' as the action name that we will pass our webhook. You can think on it as our ‘key' for the webhook. This information will let us run the right functionality in the webhook.
2.  Make sure that our @currency entity is not required. As in this codelab, we aren't going to use it, due to time limitations. However, if you wish to extend it later, you could require it and have several options to get the value of the bitcoin in different currencies.

Finally, in the ‘Fulfillment' section you need to check the ‘webhook' checkbox.

But, before you can do that, you need to click on the ‘Fulfilment' menu item and enable the ‘Webhook'.

See below:

![API.AI Image 6](https://codelabs.developers.google.com/codelabs/your-first-action-on-google-with-webhook/img/945bf91dda7364fb.png)

After you enabled it you will get the option to enter your webhook parameters.

![API.AI Image 7](https://codelabs.developers.google.com/codelabs/your-first-action-on-google-with-webhook/img/d3dfd3f6ff901e09.png)

In this phase, you can just fill the URL with: [https://us-central1-bitcoin-info-io17.cloudfunctions.net/bitcoinInfo](https://us-central1-bitcoin-info-io17.cloudfunctions.net/bitcoinInfo)

Now, click ‘Save' and return to the ‘Intents' and click on ‘Price' intnet. Scroll all the way down and you will see this new option ‘Fulfillment':

![API.AI Image 8](https://codelabs.developers.google.com/codelabs/your-first-action-on-google-with-webhook/img/e533b42f285ed51.png)

Please make sure to check the two checkboxes so API.AI will know to send its information to our webhook.

### Build the "Total" intent

Let's have another option to give the user the total number of bitcoins in the world.
Click on the + sign near the Intents menu.

![Intent Image 1](https://codelabs.developers.google.com/codelabs/your-first-action-on-google-with-webhook/img/cdbf57d194cab753.png)

And you will get an empty intent form.

Fill it like the example below.

![Intent Image 2](https://codelabs.developers.google.com/codelabs/your-first-action-on-google-with-webhook/img/3581c88e11cfd6e9.png)

You can give it a bit more examples for the ‘User says'.

(!) Don't forget to give the action name: total as it's important for our webhook in order to recognize what data to return.

Last but not least, check the ‘fulfilment' section to ‘Use webhook'. Than, please don't forget to click ‘Save'.

### Build the "Quit" intent

A good design principle is to allow our user to end the conversation.

You should click again on ‘Create Intent' button. Than, start typing few sentences that will end the conversation. For example, "bye bye" or "bye bitcoin info".

Below is how this intent should look like.

![Intent Image 3](https://codelabs.developers.google.com/codelabs/your-first-action-on-google-with-webhook/img/a0d919c198d824dc.png)

You need to check the ‘end conversation' checkbox so that it will know to really end the conversation at this point.

![Intent Image 4](https://codelabs.developers.google.com/codelabs/your-first-action-on-google-with-webhook/img/5d35edc69c242b6f.png)

> Can't see the ‘End conversation' checkbox?
>
> Please go to Integration page and enable ‘Actions on Google'.
You can see in ‘Test our action' a detailed explanation on how to do it.

### Test ‘price' intent

It's very important to check our work while we are developing it. Luckily for us, it's very easy to do it with API.AI. All you need to do after you created the new intent is to look at the right side of the screen. Type what you wish to test and you will get the response.

In the example below, we type "please tell me what is the price of bitcoin in USD" and as you can see it's working nicely, since we got a price in the response.

![Intent Image 5](https://codelabs.developers.google.com/codelabs/your-first-action-on-google-with-webhook/img/f6eb2d7035dc7636.png)