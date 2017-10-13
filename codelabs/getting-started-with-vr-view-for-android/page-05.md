# 5. Add the Google VR SDK to the project

Open app/build.gradle in Android Studio and scroll to the bottom of the file to the dependencies section and add the google vr components. The dependency section should then look like:

```
dependencies {
    compile 'com.android.support:appcompat-v7:25.1.0'
    compile 'com.android.support:design:25.1.0'

    compile 'com.google.vr:sdk-audio:1.10.0'
    compile 'com.google.vr:sdk-base:1.10.0'
    compile 'com.google.vr:sdk-common:1.10.0'
    compile 'com.google.vr:sdk-commonwidget:1.10.0'
    compile 'com.google.vr:sdk-panowidget:1.10.0'
    compile 'com.google.vr:sdk-videowidget:1.10.0'
}
```

Save the file and click the "Sync Now" alert to re-sync the Gradle project. Now we can reference the library components in our project.

> **Note**: If you get a notice about "Unregistered VCS root detected", you can click "Ignore".