# 1. Introduction

Ads are an important part of your app's overall user experience. Good ad implementations can effectively contribute to UX and even increase user retention and engagement. For example: [Rewarded Video ad](https://support.google.com/admob/answer/7372450) enables you to reward users with in-app items for watching video ads, so that users can reach new heights where otherwise they may get stuck and would have churned.

However, a great ad implementation involves of a lot of considerations, such as ad frequency, where and when to show the ad, what the reward should be, etc. On top of this, it differs from app to app, from placement to placement. There is no one-size-fits-all answer.

With Firebase Remote Config, Analytics, and AdMob, finding the sweet spot has become much easier and more streamlined. A/B testing on the fly, or progressively rolling changes out and monitoring important UX metrics, can be set up rather easily and mitigates the risk of introducing new behaviors and changes.

### What are you going to be building?

In this codelab, you're going to introduce a new ad placement variant to an existing app, set up test traffic, and roll out it progressively. We will also share with you tips about what metrics to look at, and how to turn them into actions.

<img src="https://codelabs.developers.google.com/codelabs/firebase-monetization/img/a72596c550b6f6ef.png" alt="App Image" width="300" height="500" class="medium"/>

### What you'll learn

*   How to setup test user groups with Firebase
*   How to link AdMob apps to Google Analytics for Firebase
*   How to progressively roll out new implementation with Firebase RemoteConfig
*   How to monitor key metrics of your app and turn them into actions

### What you'll need

*   [Android Studio](https://developer.android.com/sdk/installing/studio.html) version 2.3+
*   A Google account
*   A test device with Android 6.0+ with a USB cable to connect your device