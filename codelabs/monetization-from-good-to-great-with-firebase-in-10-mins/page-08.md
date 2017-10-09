# 8. Set up a 20% test with RemoteConfig in Firebase

In this step, you will create a test user group on the Firebase console that sees the new ad unit. Users in this group will see the ad unit with 30 units of reward as an experiment, and the rest of the users will still see 50 units of reward. You will then ramp this group from 20% to 100%.

### Create Remote Config parameter in Firebase

1.  Go to the [Firebase console](https://console.firebase.google.com) and select the MonetizationCodelab project you created earlier. Click **Remote Config -> Add Your First Parameter**. We create this remote parameter to dynamically change the app's behavior (in this case, the ad unit ID) for test users and default users.
2.  Name the parameter "**_game_over_rewarded_video_adunit_id_**" and set its value to the ad unit ID for the "game over 1" ad unit you created earlier.

  ![Firebase Console Image 1](https://codelabs.developers.google.com/codelabs/firebase-monetization/img/4910a8d4e002ab7d.png)

  > It usually takes a few hours for a new ad unit to be able to serve ads. In this codelab, for testing purpose, you can use **ca-app-pub-3940256099942544/6807086514** as game over 1.

  Note this is the the same as the string you passed to the Firebase Remote Config API in the previous step.

  ```java
  String adUnitID = mFirebaseRemoteConfig.getString("game_over_rewarded_video_adunit_id");
  ```

3.  Click **ADD PARAMETER** and you should see the following.

  ![Firebase Console Image 2](https://codelabs.developers.google.com/codelabs/firebase-monetization/img/a40a3cd75287069f.png)

4.  Click the parameter to trigger the edit mode, choose **Add value for condition** and then **Define new condition**.

  ![Firebase Console Image 3](https://codelabs.developers.google.com/codelabs/firebase-monetization/img/868b8402c2fa1769.png)

  ![Firebase Console Image 4](https://codelabs.developers.google.com/codelabs/firebase-monetization/img/10b98e0600c710fc.png)

5.  Name this condition "Reward Amount Test 30". Make this condition apply if "User in random percentile <= 20%" to create a randomly chosen 20% user group. Then click **CREATE CONDITION**.

  ![Firebase Console Image 5](https://codelabs.developers.google.com/codelabs/firebase-monetization/img/4df141658aad734e.png)

6.  Next, set the ad unit ID of AdMob ad unit "game over 2" as the value for "Reward Amount Test 30". Then click **UPDATE**.

  ![Firebase Console Image 5](https://codelabs.developers.google.com/codelabs/firebase-monetization/img/88e29ec105ed8414.png)

  > It usually takes a few hours for a new ad unit to be able to serve ads. In this codelab, for testing purpose, you can use **ca-app-pub-3940256099942544/8283819711** as game over 2.

Finally, push the **PUBLISH CHANGES** button to make this configuration live to the users of the app.

![Firebase Console Image 6](https://codelabs.developers.google.com/codelabs/firebase-monetization/img/dc66011d47ca870.png)