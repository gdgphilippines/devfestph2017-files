# 4. Test our action

A quick way to test how our new action works on Google home will be to use the web simulator.

We need to click on ‘Integrations' in the left side menu and then click:

1.  On the publish checkbox in the top-right side.
2.  On "Actions on Google" as this will be our integration.

![Actions on Google Image 1](https://codelabs.developers.google.com/codelabs/your-first-action-on-google-with-webhook/img/bc6255f5dda580ca.png)

Click on the **‘TEST'** button. If this is the first time you are enabling this integration, you need to click on the switch at the top-right corner to enable this integration.

The next step is to click on ‘VIEW' in order to open the Actions simulator.
You can see it in the image below.

![Actions on Google Image 2](https://codelabs.developers.google.com/codelabs/your-first-action-on-google-with-webhook/img/d8d8431b8361331f.png)

You will get a screen that lets you talk with the simulator.

![Simulator Image 1](https://codelabs.developers.google.com/codelabs/your-first-action-on-google-with-webhook/img/d148add1008c798b.png)

On the left side, you can type (or talk) your commands and on the rigth side, you can see the responses (in JSON format). Here we gave a full conversation that test our 2 intents (price and total).

> (!) If after you say: "talk to bitcoin info" the web simulator tells me the following: "Please turn on Voice & Audio Activity, Web & App Activity, and Device Information permissions for your Google Account." Go to: [https://myaccount.google.com/activitycontrols?pli=1](https://myaccount.google.com/activitycontrols?pli=1) and activate it.

We can also test our action with the automated page that API.AI created for our [new bot](https://gdev.api.ai/api-client/demo/bitcoin-info-1).

First, you need to click on ‘Deploy' so this new action will be available to the world. Then, you can customize the url so it will be easier to remember it. In our case, we typed: "bitcoin-info".

Now, you can try it.

You will see this screen:

![API.AI Image 1](https://codelabs.developers.google.com/codelabs/your-first-action-on-google-with-webhook/img/db712bf97d5a5c11.png)

It's very powerful, as you can now share your creation with the world and it will work on any device that is connected to the internet and got a browser.

**Good job!**