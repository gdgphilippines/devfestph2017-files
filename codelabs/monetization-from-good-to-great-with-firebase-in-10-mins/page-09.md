# 9. Push changes Live and monitor User Engagement Metrics

### Testing the app

Once the app is pushed live on the app store (in this codelab, compile the project and test the app on your test device), 20% of your users will see rewarded ads through "game over 2" ad unit, with a reward that hides 30% of the screen; 80% of of your users will be seeing ads through "game over 1" ad unit as before. Test your app on your test device and see which group your app instance falls in.

| Test User Group (20%) | Default User Group (80%) |
|-----------------------| ------------------------ |
|<img src="https://codelabs.developers.google.com/codelabs/firebase-monetization/img/1d81020081f750a6.png" alt="Test User Group Image" width="300" height="500" class="medium"/>|<img src="https://codelabs.developers.google.com/codelabs/firebase-monetization/img/5d6f8c9816b51474.png" alt="Default User Group Image" width="300" height="500" class="medium"/>|

### Monitor User Engagement Metrics (optional)

Goto Firebase console, from Analytics -> Dashboard -> Add Filter, choose user property reward_amount 30 as the filter, and compare this against reward_amount 50.

Look at whether the test group shows a lift or drop in terms of User Engagement (Daily Engagement, Daily Engagement per user) and in-app purchases (ARPU and ARPPU).

Since this is a codelab, you will not see data on real users and traffic, but to get a flavor of this, you can access the [Firebase demo project](https://support.google.com/firebase/answer/7157552) to see a live sample. From Analytics -> Dashboard -> Add Filter, choose different user properties, such as ad_frequency=2 and ad_frequency=3 as the filter, and see how user engagement metrics change.

### Ramp up the change from 20% to 100%

Now let's say you see good results with the new implementation and decide to roll this out to all of your users.

1.  Go to Firebase console for the project we created. Click Remote Config -> Conditions.
2.  Edit "Reward Amount Test 30" condition, change "User in random percentile <= 20%" to "User in random percentile <= 100%".

  ![Firebase Console Image](https://codelabs.developers.google.com/codelabs/firebase-monetization/img/b2aa921d301a89f8.png)

3.  Save condition and click PUBLISH CHANGES.

Now, restart your app on test device and you (as well as all your users) should see the new implementation.

| Test User Group (100%) | Default User Group (0%) |
|-----------------------| ------------------------ |
|<img src="https://codelabs.developers.google.com/codelabs/firebase-monetization/img/1d81020081f750a6.png" alt="Test User Group Image" width="300" height="500" class="medium"/>|<img src="https://codelabs.developers.google.com/codelabs/firebase-monetization/img/5d6f8c9816b51474.png" alt="Default User Group Image" width="300" height="500" class="medium"/>|

### Ramp down the change to 0%

In the other possible scenario, when you see negative results about your new implementation, and you decide to ramp down the experiment group and roll back to 100% existing implementation. Go to the same place: Remote Config -> Conditions and change "Reward Amount Test 30" condition, so that it contains "User in random percentile <= 0%". Push the change live and run the game again to see it is back to the existing implementation.

| Test User Group (0%) | Default User Group (100%) |
|-----------------------| ------------------------ |
|<img src="https://codelabs.developers.google.com/codelabs/firebase-monetization/img/1d81020081f750a6.png" alt="Test User Group Image" width="300" height="500" class="medium"/>|<img src="https://codelabs.developers.google.com/codelabs/firebase-monetization/img/5d6f8c9816b51474.png" alt="Default User Group Image" width="300" height="500" class="medium"/>|