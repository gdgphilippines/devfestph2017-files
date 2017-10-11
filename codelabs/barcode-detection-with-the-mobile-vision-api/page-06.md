# 6. Coding your Application

When you created the app with a single view activity, the template created a menu on the app. You're not going to need or use it, so find the code (in MainActivity) that handles these. These are called **onCreateOptionsMenu** and **onOptionsItemSelected**. Go ahead and delete these.

This application has a single button that will load the image, detect any faces on it, and draw a red rectangle around them when it does. Let's write the code to achieve this:


### Includes

In case you need them, here's the full set of includes that this app uses.

```java
import android.app.Activity;
import android.graphics.Bitmap;
import android.graphics.BitmapFactory;
import android.graphics.drawable.BitmapDrawable;
import android.os.Bundle;
import android.util.SparseArray;
import android.view.View;
import android.widget.Button;
import android.widget.ImageView;
import android.widget.TextView;
import com.google.android.gms.vision.Frame;
import com.google.android.gms.vision.barcode.Barcode;
import com.google.android.gms.vision.barcode.BarcodeDetector;
```

### Wiring up the Button

In your MainActivity.java in your onCreate method, add the following code:

```java
Button btn = (Button) findViewById(R.id.button);
btn.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View v) {
        }
});
```

This sets up the event handler (onClick) for when the user presses the button. When they do that, we want to load the bar code, process it for data, and write that data to the TextView.

### Load the Image

Let's start with loading the image. We do this first by getting a reference to our image view, that we call ‘myImageView'. Then you use a BitMapFactory to decode the R.drawable.puppy resources into a Bitmap. Then you set it to be the bitmap for myImageView.

```java
ImageView myImageView = (ImageView) findViewById(R.id.imgview);
Bitmap myBitmap = BitmapFactory.decodeResource(
                        getApplicationContext().getResources(), 
                        R.drawable.puppy);
myImageView.setImageBitmap(myBitmap);
```

### Setup the Barcode Detector

Next we'll setup the detector we're going to use to detect a barcode.

We create our new BarcodeDetector using a builder, and tell it to look for QR codes and Data Matrices (there are a lot of other barcode types we could also look for).

It's possible that, the first time our barcode detector runs, Google Play Services won't be ready to process barcodes yet. So we need to check if our detector is operational before we use it. If it isn't, we may have to wait for a download to complete, or let our users know that they need to find an internet connection or clear some space on their device.

```java
BarcodeDetector detector = 
    new BarcodeDetector.Builder(getApplicationContext())
                        .setBarcodeFormats(Barcode.DATA_MATRIX | Barcode.QR_CODE)
                        .build();
if(!detector.isOperational()){
   txtView.setText("Could not set up the detector!");
   return;
}
```

### Detect the Barcode

Now that our detector is set up and we know it's operational, we'll detect the barcode. This code is pretty straightforward -- it creates a frame from the bitmap, and passes it to the detector. This returns a SparseArray of barcodes.

Note that the API is capable of detecting multiple bar codes in the same frame. In this case our entire image is a bar code, but consider what would happen with a camera preview type app where you are looking at multiple bar codes -- in this case the SparseArray<Barcode> would be populated with multiple entries.

```java
Frame frame = new Frame.Builder().setBitmap(myBitmap).build();
SparseArray<Barcode> barcodes = detector.detect(frame);
```

### Decode the Barcode

Typically in this step you would iterate through the SparseArray, and process each bar code independently. Usually, we need to allow for the possibility that there won't be any barcodes, or there might be several. Because for this sample, I know I have 1 and only 1 bar code, I can hard code for it. To do this, I take the Barcode called ‘thisCode' to be the first element in the array. I then assign it's rawValue to the textView -- and that's it -- it's that simple!

```java
Barcode thisCode = barcodes.valueAt(0);
TextView txtView = (TextView) findViewById(R.id.txtContent);
txtView.setText(thisCode.rawValue);
```
