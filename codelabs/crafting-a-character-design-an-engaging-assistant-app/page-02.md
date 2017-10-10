# 2. Design process

As you get started, familiarize yourself with our recommended [design process methodology](https://developers.google.com/actions/design/principles). The main steps are:

*   **Pick the right use cases.** Use cases that work well with voice interfaces are usually simple, intuitive, and leverage top-of-mind answers or hands-busy situations.
*   **Create a persona.** The way your Assistant app talks and behaves will give it consistency, unique brand presence, and personality.
*   **Write dialogs.** Getting multiple and separate dialogs that users can potentially go through down ahead of time will give you reliable guard rails for development.
*   **Test it out.** Say it out loud, test it in our [simulator](https://developers.google.com/actions/tools/web-simulator) tools, and make sure it sounds conversational.
*   **Build and iterate.** Build it in [API.AI](https://docs.api.ai/) or with your own tooling using the Actions SDK.

### What is an Assistant app?

With Assistant apps, you can create and integrate your services with the Google Assistant. They are your way to fulfill user requests by enabling a two-way dialog with them. When users request an action, the Google Assistant processes this request, determines the best action to invoke, and invokes your Assistant app if relevant. From there, your action manages the rest, including how users are greeted, how to fulfill the user's request, and how the conversation ends.

> **Looking for more? Check out the** [**Actions on Google developer site.**](https://developers.google.com/actions/develop/conversation)

### What you will design

In this codelab, you're going to design a character and the dialog for an Assistant app for the Google Assistant, including:

*   Defining the characteristics of your persona
*   Greeting the user in character
*   Creating a multi-step conversation
*   Correct and incorrect responses and "back on track" repair strategies
*   Completing the quiz
*   Getting ready to build your new Assistant app 

![Google Assistant App Image](https://codelabs.developers.google.com/codelabs/conversation-design/img/26c8e0f46803309e.png)

### What you'll learn

*   How to design a recognizable persona
*   How to design a multi-step conversation
*   How to prepare your Assistant app to build using API.ai