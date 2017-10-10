# 3. Configure Build.gradle

In this step you'll ensure that your app can use Google Play services, in which the Mobile Vision APIs reside. To do this, you'll first update your build.gradle file.

In Android Studio, open the Gradle Scripts node, and select build.gradle (Module App) as shown:

![Gradle Scripts Node Image](https://codelabs.developers.google.com/codelabs/barcodes/img/830e08c9dba7e559.png)

This will open your build.gradle file, at the bottom of which will be code like this:

```java
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
}
```

Add a dependency for play services like this:

```java
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.android.support:appcompat-v7:22.0.0'
    compile 'com.google.android.gms:play-services:7.8+'
}
```

If you are asked to perform a gradle sync, do so. Otherwise, find the Gradle Sync button on the toolbar and press it to trigger a sync. It looks like this:

![Sync Project with Gradle FIles Image](https://codelabs.developers.google.com/codelabs/barcodes/img/9b2639b1071d00f3.png)



