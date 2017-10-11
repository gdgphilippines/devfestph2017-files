# 4. Set up Rewarded Video Ad Unit

Next, create an app and ad unit as the following.

> It usually takes a few hours for a new ad unit to be able to serve ads. In this codelab, for testing purpose, you can use **ca-app-pub-3940256099942544/6807086514** which has exactly the same settings as described below.

1.  Go to the [AdMob front end](https://apps.admob.com/).
2.  If you have already upgraded to [AdMob beta](https://support.google.com/admob/answer/7367022?hl=en&ref_topic=7356215) or are using a new AdMob account, from **Apps** menu, click "**Add App**", answer "**NO**" when asked "Have you published your app on Google Play or the App Store," then name the app "MonetizationCodelab", choose "Android" as the Platform, and click "**Add**". You should see the following screen as a result.

  > If you have not upgraded to AdMob beta yet, find how to do so [here](https://support.google.com/admob/answer/7367022?hl=en&ref_topic=7356215). If your account is not yet eligible for the upgrade, for this code lab, you can use the test ad unit provided above, or create a new account. Note that without upgrading to AdMob beta you can still create the app and ad unit with the same settings under the MONETIZATION tab, but the UI flow will be different from the descriptions in this codelab.

  ![Monetization Image 1](https://codelabs.developers.google.com/codelabs/firebase-monetization/img/6960e47cc77a8342.png)

3.  Click **NEXT: CREATE AD UNIT**, select **REWARDED VIDEO**, set reward amount to 50, and reward item to "hint" (these are the reward the app current gives to users), name it "game over 1", and save.

  ![Monetization Image 2](https://codelabs.developers.google.com/codelabs/firebase-monetization/img/6985564a5ff135a0.png)

4.  When successfully saved, you will see the ad unit id as the following. Modify the `GameActivity.java` file as the following so that the app refers to the ad unit we just created. (Change the `ca-app-pub-3940256099942544/6807086514` part with the ad unit id you just created.)

  ![Monetization Image 2](https://codelabs.developers.google.com/codelabs/firebase-monetization/img/a2e7230d5296d8c7.png)

### GameActivity.java

```java
private static final String DEFAULT_AD_UNIT_ID = "ca-app-pub-3940256099942544/6807086514";
```