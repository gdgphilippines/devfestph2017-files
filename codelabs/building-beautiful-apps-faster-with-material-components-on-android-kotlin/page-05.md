# 5. Two-by-two

To create the card layout with increased density, move to a GridLayoutManager for the *RecyclerView*. This card layout also supports large screens.

![Shrine App Sceenshot](https://codelabs.developers.google.com/codelabs/mdc-android-kotlin/img/65a9ebc87a9df332.png)

***

### Add resource values to define column width
You can select different values based on screen size with Android resources. Use Android resources to pick a different number of cards for smaller and larger screens.

Add a new integers.xml file in app's values resources folder:

app/res/values/integers.xml

```xml
<resources>
    <integer name="shr_column_count">2</integer>
</resources>
```

This new integer resource will be the default column count, if none of the other possible qualifiers apply. Next, add some special qualifier values for larger screens. Add another integers.xml file with a qualifier for Screen Width >= 480dp:

app/res/values-w480dp/integers.xml

```xml
<resources>
    <integer name="shr_column_count">3</integer>
</resources>
```

If the screen width ever grows larger than 480dp in landscape mode, the grid will display 3 columns instead of 2.

Add one for 960dp as well:

app/res/values-w960dp/integers.xml

```xml
<resources>
    <integer name="shr_column_count">6</integer>
</resources>
```

This will take care of devices that are much larger than the base size, such as large tablets.

### Use GridLayout manager in the RecyclerView
Adjust `MainActivity` to use a `GridLayoutManager` and initialize it with the column count from the resources. Update `MainActivity`'s onCreate to replace its `LinearLayoutManager` with a `GridLayoutManager`:

app/java/MainActivity.kt

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
/* ... */

    product_list.setHasFixedSize(true)
    product_list.layoutManager = GridLayoutManager(this, resources.getInteger(R.integer.shr_column_count))
    adapter = ProductAdapter(products, imageRequester)
    product_list.adapter = adapter

/* ... */
}
```

Run the app:

*GridLayoutManager* makes it easy to create a layout with items of constant size.
Loading the column count from resources will take care of changing the density of content as screen size changes, such as when the user rotates their device or adjusts the size of an application in multi-window mode.
Try the app across rotations or maybe on a larger device or emulator to see how the layout changes.

![Shrine App Screenshot](https://codelabs.developers.google.com/codelabs/mdc-android-kotlin/img/62c118c0561aeaf1.png)

***

> To learn more about collections (lists) in design and app flows, see the [Material Design guidelines](https://material.io/guidelines/components/lists.html#lists-actions).