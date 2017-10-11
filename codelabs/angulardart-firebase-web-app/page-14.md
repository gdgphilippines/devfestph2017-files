# 14. What's next?
Congratulations! You have successfully implemented a simple shared counter using the AngularDart framework and the Firebase Realtime Database.

## Interested in learning more?

- **Run your app in any browser:** You've been developing in Dartium, but if you open your app in any other browser, it'll work the same. It'll just need a bit more time at first to compile to JavaScript.

- **Configure** `dartanalyzer`: WebStorm and other tools use dartanalyzer to statically check your code for any potential problems. You can configure the analyzer using the `analysis_options.yaml` file. [Learn more](https://www.dartlang.org/guides/language/analysis-options).

- **Learn about other Firebase features**: Firebase is not just a realtime database. You can also use authentication, storage, and other features. [Learn more](https://firebase.google.com/features/).

- **Build the app:** Use `pub build` when you're ready to deploy your web app to production. [Learn more](https://webdev.dartlang.org/tools/pub/pub-build).

- **Deploy the app:** Firebase Hosting is an easy way to deploy your application. [Learn more](https://firebase.google.com/docs/hosting/).

- **Restrict access to member variables:** For brevity, this codelab hasn't touched Dart's privacy functionality. You might want to make `ref` and `updateDatabase` private members of `AppComponent`. Use WebStorm's refactoring functionality and simply rename them to `_ref` and `_updateDatabase`. Learn more about [Dart privacy](https://www.dartlang.org/guides/language/language-tour#libraries-and-visibility) or [WebStorm features](https://www.jetbrains.com/webstorm/features/).

- **Test the app:** Use the angular_test package to perform component/subsystem testing. [Learn more](https://webdev.dartlang.org/angular/guide/testing).

## Resources
- Dart official website: [www.dartlang.org](https://www.dartlang.org/)
- AngularDart official website: [webdev.dartlang.org/angular](https://webdev.dartlang.org/angular)
- Firebase official website: [firebase.google.com](https://firebase.google.com/)
- Dart Firebase library: [pub.dartlang.org/packages/firebase](https://pub.dartlang.org/packages/firebase)
- Community and support: [www.dartlang.org/community](https://www.dartlang.org/community)