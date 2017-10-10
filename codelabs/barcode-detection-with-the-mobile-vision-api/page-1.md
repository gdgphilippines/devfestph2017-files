# 1. Overview

With the release of Google Play services 7.8 we're excited to announce that we've added new Mobile Vision APIs which provide bar code detection APIs that read and decode a myriad of different bar code types, quickly, easily and locally.


### Barcode detection

Classes for detecting and parsing bar codes are available in the com.google.android.gms.vision.barcode namespace. The BarcodeDetector class is the main workhorse -- processing Frame objects to return a SparseArray<Barcode> types.

The Barcode type represents a single recognized barcode and its value. In the case of 1D barcode such as UPC codes, this will simply be the number that is encoded in the bar code. This is available in the rawValue property, with the detected encoding type set in the format field.

![Barcode Image](https://codelabs.developers.google.com/codelabs/barcodes/img/ef7d048daa59743.png)

For 2D bar codes that contain structured data, such as QR codes -- the valueFormat field is set to the detected value type, and the corresponding data field is set. So, for example, if the [URL](https://developers.google.com/android/reference/com/google/android/gms/vision/barcode/Barcode.html#constants) type is detected, the constant URL will be loaded into the valueFormat, and the [Barcode.UrlBookmark](https://developers.google.com/android/reference/com/google/android/gms/vision/barcode/Barcode.UrlBookmark) will contain the URL value. Beyond URLs, there are lots of different data types that the QR code can support -- check them out in the documentation [here](https://developers.google.com/android/reference/com/google/android/gms/vision/barcode/Barcode).

When using the Mobile Vision APIs, you can read barcodes in any orientation - they don't always need to be the straight on, and oriented upwards!

Importantly, all bar code parsing is done locally, so you don't need to do a server round trip to read the data from the code. In some cases, such as [PDF-417](https://en.wikipedia.org/wiki/PDF417), which can hold up to 1kb of text, you may not even need to talk to a server at all to get all the information you need.

You can learn more about using the API by checking out the Multi-Detector sample on GitHub. This uses the Mobile Vision APIs along with a Camera preview to detect both faces and bar codes in the same image.


### Prerequisites

Before beginning, check that you have all the necessary pre-requisites. These include:

- Android Studio
- An Android Device that runs Android 4.2.2 or later -or- A configured Android Emulator (this is available in Android Studio)
- The latest version of the Android SDK including the SDK tools component. You can get this from the Android SDK Manager in Android Studio.
- The Google Play Services SDK. You can get this from the Android SDK Manager in Android Studio.	