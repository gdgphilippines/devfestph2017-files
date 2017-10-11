# 6. Modify the reward

### Add a variant Rewarded Video implementation

You now have a reward mechanism in the form of a hint, but you're not sure how a change in the amount of the hint will impact user engagement and gameplay experience in your app. Therefore, you want to be careful and test on a small group of users first. Does the reward still enable users to reach new heights without making it too easy or too difficult? We'll gather data to answer this question.

1.  First, create another Rewarded Video ad unit as a test ad unit. From Apps Menu, click **Apps**, choose MonetizationCodelab (the app you created earlier), click **Ad units**, and click the **ADD AD UNIT** button.

  > It usually takes a few hours for a new ad unit to be able to serve ads. In this codelab, for testing purpose, you can use **ca-app-pub-3940256099942544/8283819711** which has exactly the same settings as described below.

  ![AdMob Image 1](https://codelabs.developers.google.com/codelabs/firebase-monetization/img/8846ad05682cb437.png)

2.  Choose Rewarded video, set reward amount to 30, and reward item to "hint". The reward amount is the percent of the screen that will be covered in your hint. Note that for this new ad unit, the reward amount value is different from that of the existing ad unit. Name the ad unit "game over 2" and save.

  ![AdMob Image 2](https://codelabs.developers.google.com/codelabs/firebase-monetization/img/51b6d9ae2041e470.png)

That's it on the AdMob front end.