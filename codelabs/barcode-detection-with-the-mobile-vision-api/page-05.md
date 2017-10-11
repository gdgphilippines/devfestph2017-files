# 5. Adding a UI to your App


Now that your app is fully configured, it's time to build a UI that lets the user detect a face in an image, and then overlay that face with a bounding box.

In Android Studio, select the **‘res'** folder, and open its **‘layout'** subfolder. In here you'll see ‘`activity_main.xml`'.

Double click to open it in the editor, and be sure to select the **‘Text'** tab at the bottom of the editor to get the XML text view of your Layout. Android Studio Should look something like this:

![Editor Image](https://codelabs.developers.google.com/codelabs/barcodes/img/78f44ebf191edd14.png)

You can see that your layout contains a single <mark><TextView></mark> node. Delete this and replace with:

```java
<LinearLayout
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:orientation="vertical">

    <TextView android:text=""
        android:layout_width="wrap_content" 
        android:layout_height="wrap_content"
        android:id="@+id/txtContent"/>
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Process"
        android:id="@+id/button"
        android:layout_alignParentTop="true"
        android:layout_alignParentStart="true" />
    <ImageView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/imgview"/>
</LinearLayout>
```

This layout gives you a button which will loading and then processing an image, which will appear in the ImageView. After processing is complete, the data from the Barcode will render in the TextView.

### Adding a Barcode to your app


Typically you would take pictures of bar codes with the device's camera, or maybe process the camera preview. That takes some coding, and in later steps you'll see a sample that does this. To keep things simple, for this lab, you're just going to process an image that is already present in your app.

Here's the image:


![QR Code](https://codelabs.developers.google.com/codelabs/barcodes/img/bb74793e6be9106b.png)

Name it puppy.png, and add it to the res/drawable directory on your file system. You'll see that Android Studio adds it to the drawable directory. It also makes the file accessible as a resource, with the following ID: `R.drawable.puppy`

With the image in place, you can now begin coding your application.
