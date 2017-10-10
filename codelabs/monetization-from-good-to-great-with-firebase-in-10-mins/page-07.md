# 7. Wire the code up in Android Studio

Now we have a separate Rewarded Video ad unit that gives a hint that covers less of the screen. The next thing you will do is to set up an experiment so that you can progressively ramp up the new ad unit and sunset the existing one, while monitoring user engagement metrics. This experiment involves changes to both your project code and the Firebase console.

To achieve this, you will change your project code so that the app retrieves the ad unit ID dynamically. On the Firebase console, you will create a test user group that sees the new ad unit, and ramp this group up from 20% of all users to 100%.

### Modify build.gradle

1.  Update your app module `build.gradle` file. This is `app/build.gradle` in your project. Add the following line to your dependencies block.

  ```
  compile 'com.google.firebase:firebase-config:10.2.1'
  ```

2.  Sync Project with Gradle Files (click Sync Now in the IDE prompt.)

  ![Android Studio IDE Prompt Image](https://codelabs.developers.google.com/codelabs/firebase-monetization/img/d12a2a42197be233.png)

### Initialize Firebase Components

1.  As you will work with Google Analytics for Firebase and Remote Config in multiple points, in your code (e.g. `GameActivity` and `FailActivity`), create references to the singleton Remote Config object and Firebase Analytics object, as shown in the following.

  #### FailActivity.Java

  ```java
  public class FailActivity extends AppCompatActivity {
     ...
     private FirebaseAnalytics mFirebaseAnalytics;
     ...
     protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          mFirebaseAnalytics = FirebaseAnalytics.getInstance(this);

     ...
  }
  ```

  #### GameActivity.Java

  ```java
  public class GameActivity extends AppCompatActivity {
     ...
     private FirebaseAnalytics mFirebaseAnalytics;
     private FirebaseRemoteConfig mFirebaseRemoteConfig;
     ...
     protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          mFirebaseAnalytics = FirebaseAnalytics.getInstance(this);
          mFirebaseRemoteConfig = FirebaseRemoteConfig.getInstance();
     ...
  }
  ```

2.  Enable developer mode for RemoteConfig for test purposes.

  #### GameActivity.Java

  ```java
  @Override
  protected void onCreate(Bundle savedInstanceState) {
     ...
     mFirebaseRemoteConfig = FirebaseRemoteConfig.getInstance();
     FirebaseRemoteConfigSettings configSettings = new FirebaseRemoteConfigSettings.Builder()
                 .setDeveloperModeEnabled(true)
                 .build();
     mFirebaseRemoteConfig.setConfigSettings(configSettings);
     ...
  }
  ```

  > **Tip**: Normally requests to Remote Config server is limited at a few requests per hour. Enabling developer mode can bypass this limit for testing purposes. Please find more details in the [Firebase FAQ](https://firebase.google.com/support/faq/#remote-config-requests).

3.  Next, enable in-app default values for RemoteConfig. Set the in-app default values for RemoteConfig variables so that even if the device is offline, the app can still run. In this example, you will set a default for the ad unit ID in the `onCreate` method in `GameActivity.java`.

  #### GameActivity.java

  ```java
  protected void onCreate(Bundle savedInstanceState) {
     ...
     mFirebaseAnalytics = FirebaseAnalytics.getInstance(this);
     mFirebaseRemoteConfig = FirebaseRemoteConfig.getInstance();
     FirebaseRemoteConfigSettings configSettings = new FirebaseRemoteConfigSettings.Builder()
             .setDeveloperModeEnabled(true)
             .build();
     mFirebaseRemoteConfig.setConfigSettings(configSettings);

     HashMap<String, Object> defaults = new HashMap<>();
     defaults.put("game_over_rewarded_video_adunit_id", DEFAULT_AD_UNIT_ID);
     mFirebaseRemoteConfig.setDefaults(defaults);
  ```

### Log Events in code

Google Analytics for Firebase automatically logs a number of generic events. In order to measure how many users have cleared the game or failed the game, you will log two more custom events - stage_clear and stage_failed, so that you can evaluate the experiment. You will log the events using the following names.

*   stage_clear
*   stage_failed

Go to GameActivity.java and add the following code.

1.  Add a logEvent call as the following in the _clearStage()_ method.

  ```java
  private void clearStage () {
     mFirebaseAnalytics.logEvent("stage_clear", null);
  ...
  ```

2.  Add a logEvent call as the following in the _failStage()_ method.

  ```java
  private void failStage () {
     mFirebaseAnalytics.logEvent("stage_failed", null);
  ...
  ```

### Fetch values from server

1.  Go to GameActivity.java and create a new method that fetches and applies all server-side parameters. In a later step in this codelab, you will define a server-side parameter for the ad unit ID.

  #### GameActivity.java

  ```java
  private void fetchSettings() {
     OnCompleteListener<Void> onCompleteListener = new OnCompleteListener<Void>() {
         @Override
         public void onComplete(@NonNull Task<Void> task) {
             if (task.isSuccessful()) mFirebaseRemoteConfig.activateFetched();
         }
     };

     if (mFirebaseRemoteConfig.getInfo().getConfigSettings().isDeveloperModeEnabled()) {
         // This forces Remote Config to fetch from server every time.
         mFirebaseRemoteConfig.fetch(0).addOnCompleteListener(this, onCompleteListener);
     } else {
         mFirebaseRemoteConfig.fetch().addOnCompleteListener(this, onCompleteListener);
     }
  }
  ```

2.  Fetch the RemoteConfig values at onCreate (in FailActivity), as the following. Keep in mind that once the values are fetched, they will be cached. See [help center article](https://firebase.google.com/docs/remote-config/use-config-android) for more details. In this codelab, you will fetch the value only within `onCreate`. Also note that, since `fetchSettings()` refers to `mFirebaseRemoteConfig`, make sure it is called after `mFirebaseRemoteConfig` is initialized and developer mode is enabled.

  #### GameActivity.java

  ```java
  protected void onCreate(Bundle savedInstanceState) {
      ... 
      fetchSettings();
  }

  ```

### Apply Remote Config values to app logic

Next, when requesting ads, instead of using the static DEFAULT_AD_UNIT_ID constant, use the the ad unit id remotely fetched, and apply it to the ad request.

```java
String adUnitID = mFirebaseRemoteConfig.getString("game_over_rewarded_video_adunit_id");
...
mAd.loadAd(
   adUnitID,
   new AdRequest.Builder().build()
);
...
```

### User Property Tracking

To help Firebase Analytics record the setting for the reward issued to this user, store the reward value into a user property so that gets associated with all of this user's events.

Add the following code in the `onRewarded` method in FailActivity.

#### FailActivity.java

```java
public void onRewarded(RewardItem reward) {
   mRewardItem = reward;
   mFirebaseAnalytics.setUserProperty(
      "reward_amount",
      new Integer(reward.getAmount()).toString()
   );
}
```