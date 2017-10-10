# 3. Create your first Agent

After the login you can create your first agent.
See below:

![API.AI Image 1](https://codelabs.developers.google.com/codelabs/your-first-kids-action-on-google/img/79fc3a5e575ae691.png)

You will need to:

1.  Give your agent a name.
    In our case, it will be "AnimalJoker". Please note that the agent name can not contain any spaces between the words.
2.  Give a short description so other users will know what this action is going to do.
    In our case, type: "An action that tells animal jokes. But only the good ones".
3.  Click on ‘Save'.
    It's the button in the top-right corner of the screen.

### Create our first Entity

#### What are entities?

Entities are the values we are trying to capture from the user phrases. Kind of like filling out a form, requesting details from the user. API.AI looks to extract these out, and will do follow up prompts until done.

This is how an entity looks in API.AI

![Entity Image 1](https://codelabs.developers.google.com/codelabs/your-first-kids-action-on-google/img/f147fd2e217ff232.png)

We will create an Animal entity.

#### Create an Animal entity

First step is to click on ‘Entities' menu item on the left and then on ‘Create Entity' button.

![API.AI Image 2](https://codelabs.developers.google.com/codelabs/your-first-kids-action-on-google/img/4c50acb3fc2ce5cd.png)

Next you should start typing animals' names. Don't forget to give our entity a name (e.g. Animal) and click on the ‘save' button after few animal's names.

The final results should look similar to the image below.

![API.AI Image 3](https://codelabs.developers.google.com/codelabs/your-first-kids-action-on-google/img/280c8dc4c1f04cd6.png)

#### Important aspects to note:

1.  You should ‘help' our machine learning algorithm to train itself by providing synonyms. For example, **dog** could also be **puppy**. In our case, you can give it only 2-3 animals. That will be fine for now. You don't need to specify plurals as our machine learning is doing it for you.
2.  In the real world, try to give many examples so it will cover more cases.
3.  Click on the ‘save' button so your hard work will be saved.

### Create our intents

#### What is an intent?

An Intent is triggered by a series of "user says" phrases. This could be something like "please tell me an animal joke" or "Give me a recipe for burger".

You need to specify enough sentences to train API.AI's machine learning algorithm. Then even if the user doesn't say exactly the words you typed here, API.AI can still understand them!

![API.AI Image 4](https://codelabs.developers.google.com/codelabs/your-first-kids-action-on-google/img/5f8a76b120f219a9.png)

You should create separate intents for different types of actions though.

Don't try to combine all of them together.

In our example, we will create only two intents:

*   **Tell_Joke intent** - This intent will handle the jokes.
*   **Quit intent** - This intent will handle the part when the user wish to finish the action.

#### Build the "Tell_Joke" intent

After we have our new $Animal entity. If you notice the $ before the word - It's not a mistake. This is the way we will refer to our new entity from now. Think of it as a special sign to show us that we are referring to our entity and not just another animal.

It's time to create the intent that will tell us the jokes.

First, click on ‘Intents' in the left menu and then on the ‘Create Intent' button.

![API.AI Image 5](https://codelabs.developers.google.com/codelabs/your-first-kids-action-on-google/img/248d5d1bc0153a0c.png)

Second, start typing few sentences that you will want to use to get a joke. For example, "please tell me a joke about dogs". Type a few sentences so API.AI could start training its algorithms. You can see that while you type, API.AI automatically recognizes that the phrase includes one of the entities, so it highlights it. Next, give your intent a name (e.g. tell_joke) and hit the ‘save' button.

See below how it should look like.

![API.AI Image 6](https://codelabs.developers.google.com/codelabs/your-first-kids-action-on-google/img/9a99b648a767d54e.png)

We will skip the 'Events' section for now. In the 'Action' section we need to make sure that our @Animal entity is marked Required. In the Prompts we should type "Please tell me which animal you like" (see the screenshot below), so in cases where the user didn't name an animal, it will be clear to her that we need this entity.

Finally, we will fill ‘Text Response' section with our most amazing jokes. You can take few ideas from the image below or type one or two good jokes that you know.

Please note that we are using the **$Animal** value in our response in order to create a joke that is based on the animal that the user asked. The **$** sign is to mark that we are working with a variable. The **@** sign is to identify an entity.

![API.AI Image 7](https://codelabs.developers.google.com/codelabs/your-first-kids-action-on-google/img/5f3f37091810d228.png)

After you fill all your amazing jokes, don't forget to click on the ‘save' button on the top-right corner of the screen.

### Build the "Quit" intent

A good design principle is to allow our user to end the conversation.

Click on the ‘Create Intent' button again. Then, start typing few sentences that will end the conversation. For example, "bye bye" or "bye animal joker". Make sure to give the intent a name at the top of the screen (e.g Quit). Below is an example of what this intent should look like.

![API.AI Image 8](https://codelabs.developers.google.com/codelabs/your-first-kids-action-on-google/img/790843799487442e.png)

Last, but not least, you need to check the ‘end conversation' checkbox so that it will know to really end the conversation at this point.

![API.AI Image 9](https://codelabs.developers.google.com/codelabs/your-first-kids-action-on-google/img/31abfe6cb9c1a4de.png)

> Can't see the ‘End conversation' checkbox?
>
> Please go to Integration page and enable ‘Actions on Google'. You can see in ‘Test our action' a detailed explanation on how to do it.

### Test Tell_Joke intent

It's very important to check our work while we are developing it. Luckily for us, it's very easy to do it with API.AI. All you need to do after you created the new intent is to look at the right side of the screen. Type what you wish to test and you will get the response.

In the example below, we type "please tell me a joke about dino" and as you can see it's working nicely, since we got a joke in the response.

![Tell Joke Intent Image](https://codelabs.developers.google.com/codelabs/your-first-kids-action-on-google/img/264e28ce03390e96.png)