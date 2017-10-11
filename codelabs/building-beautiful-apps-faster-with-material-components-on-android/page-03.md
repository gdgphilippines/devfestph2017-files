# 3. Add a Collapsing App Bar

### What's a Collapsing App Bar?
Let's say the designer gave you the following mocks and you're going to update the app to match.

![Shrine App Screenshot 1](https://codelabs.developers.google.com/codelabs/mdc-android/img/ba9916c0d6d5e9dd.png)
![Shrine App Screenshot 2](https://codelabs.developers.google.com/codelabs/mdc-android/img/65a9ebc87a9df332.png)
![Shrine App Screenshot 3](https://codelabs.developers.google.com/codelabs/mdc-android/img/88254bee646c4af8.png)

Making a static layout that looks like the mock on the left can be done with simple layout classes built into the Android framework, and the mock on the right can be done with the Toolbar class, but how do you transition between those two states and handle the gestural input properly?

`AppBarLayout`, together with `CollapsingToolbarLayout` and `CoordinatorLayout`, can do that: `CoordinatorLayout` notifies its children of scroll events and changing positions, `AppBarLayout` provides configurable behavior based on scroll position, and `CollapsingToolbarLayout` allows you to configure how views move between an expanded and collapsed size. When it gets to a minimum height, say 56dp, it freezes in place and allows the scrolling `View` (such as a `NestedScrollView` or `RecyclerView`) to scroll beneath it! It even adds a shadow.

### Add a Material Components for Android to your project

In the `app` module's `build.gradle` file, update the dependency section to add a dependency for Material Components for Android:

app/build.gradle

```
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])

    compile 'com.android.support:appcompat-v7:25.3.1'
    compile 'com.android.support:cardview-v7:25.3.1'
    compile 'com.android.support:recyclerview-v7:25.3.1'
    compile 'com.android.volley:volley:1.0.0'
    compile 'com.google.code.gson:gson:2.2.4'

    // Add this:
    compile 'com.android.support:design:25.3.1'
}
```

After editing the file, make sure to sync the project to these changes so that all the necessary libraries can be installed. This can be done by choosing "Sync Project with Gradle Files" from the Tools -> Android menu.

![Tools Image](https://codelabs.developers.google.com/codelabs/mdc-android/img/a0601b977252bc86.png)

***

### Add a CoordinatorLayout to MainActivity
`CoordinatorLayout` acts like a `FrameLayout`, but allows its children to react to changes in dependent views, such as scrolling or other movement. To take advantage of these features, use a `CoordinatorLayout` as the top-level container.

Replace the `FrameLayout` in MainActivity's layout file with a `CoordinatorLayout`:

app/res/layout/shr_main.xml

```xml
<android.support.design.widget.CoordinatorLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <!-- ... -->

</android.support.design.widget.CoordinatorLayout>
```

Make sure that you include the `xmlns:app` attribute when making this replacement, as we'll need the `app` namespace to use attributes from these widgets.


### Wrap the Toolbar in an AppBarLayout and CollapsingToolbarLayout
As the main content moves, `AppBarLayout` and `CollapsingToolbarLayout` will work with the scrolling RecyclerView to adjust toolbar content.

Wrap the existing `Toolbar` tag in `MainActivity's` layout file with a `CollapsingToolbarLayout` inside of an `AppBarLayout`:

app/res/layout/shr_main.xml

```xml
<!-- ... -->
<android.support.design.widget.AppBarLayout
    android:layout_width="match_parent"
    android:layout_height="400dp">

    <android.support.design.widget.CollapsingToolbarLayout
        android:id="@+id/collapsing_toolbar"
        style="@style/Widget.Shrine.CollapsingToolbar"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_scrollFlags="scroll|exitUntilCollapsed|snap">


        <!-- Wrap this view: -->
        <android.support.v7.widget.Toolbar
            android:id="@+id/app_bar"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"/>

    </android.support.design.widget.CollapsingToolbarLayout>

</android.support.design.widget.AppBarLayout>
<!-- ... -->
```

The `AppBarLayout's` height determines the height of the app bar in its expanded state. The `Toolbar's` height determines the height of the app bar in its collapsed state. In this case, this means that our app bar will collapse from 400dp to actionBarSize (56dp on smaller devices like phones, 64dp on larger devices like tablets).

The `layout_scrollFlags` tell the `AppBarLayout` how that child should behave during scrolling. In this case, `layout_scrollFlags` specifies that scrolling events will be consumed by this View until the `CollapsingToolbarLayout` reaches its minimum height, and that it should snap to be fully expanded or fully collapsed once scrolling stops (and never be stuck in a middle state).

There's another detail we need to get the scroll behavior correct: specify to the `CollapsingToolbarLayout` that you want to keep the `Toolbar` itself on the screen at all times. Add a `layout_collapseMode` attribute to the `Toolbar` tag:

app/res/layout/shr_main.xml

```xml
<!-- ... -->
<!-- Add attribute to this view: -->
<android.support.v7.widget.Toolbar
    android:id="@+id/app_bar"
    android:layout_width="match_parent"
    android:layout_height="?attr/actionBarSize"
    app:layout_collapseMode="pin"/>
<!-- ... -->
```

One last thing: tell `CoordinatorLayout` that the `RecyclerView's` scroll events should notify `AppBarLayout`. `CoordinatorLayout Behaviors` can respond to a number of different user interactions, including scrolls and flings (very fast scrolling). We need a `Behavior` implementation that intercepts scrolling events and forwards them to our AppBarLayout for handling, so that it can potentially use them to expand or collapse our app bar. Fortunately, there is a Behavior implementation written just for this purpose: `appbar_scrolling_view_behavior`. Add this `Behavior` implementation to the `RecyclerView` in `MainActivity's` layout file and remove the top margin. `CoordinatorLayout` will handle the vertical offset automatically.

app/res/layout/shr_main.xml

```xml
<!-- ... -->
<!-- Add attribute to this view: -->
<android.support.v7.widget.RecyclerView
    android:id="@+id/product_list"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:layout_marginLeft="@dimen/shr_list_margin"
    android:layout_marginRight="@dimen/shr_list_margin"
    app:layout_behavior="@string/appbar_scrolling_view_behavior"/>
<!-- ... -->
```

Remove the v21-specific styles for the `Toolbar`. `CollapsingToolbarLayout` will handle adding the necessary elevation as the toolbar is collapsed. Delete the `Widget.Shrine.Toolbar` style from the `values-v21/styles.xml` file. Note that in Android Studio, all the `styles.xml` files will be grouped under a single entry. You can expand this entry to select the `(v21)` version and make this change on it.

app/res/values-v21/styles.xml

```xml
<!-- ... -->
<!-- Delete this v21 style override: -->
<!--
<style name="Widget.Shrine.Toolbar" parent="Widget.AppCompat.Toolbar">
    <item name="android:background">?attr/colorPrimary</item>
    <item name="android:elevation">4dp</item>
    <item name="contentInsetStart">12dp</item>
    <item name="titleTextAppearance">@style/TextAppearance.Shrine.Logo</item>
</style>
-->
<!-- ... -->
```


Remove the background color from the main `Toolbar` styles, as it will interfere with the collapse animation:

app/res/values/styles.xml

```xml
<!-- ... -->
<style name="Widget.Shrine.Toolbar" parent="Widget.AppCompat.Toolbar">
    <!-- Delete this style item: -->
    <!--
    <item name="android:background">?attr/colorPrimary</item>
    -->
    <item name="contentInsetStart">12dp</item>
    <item name="titleTextAppearance">@style/TextAppearance.Shrine.Logo</item>
</style>
-->
<!-- ... -->
```


Uncomment the predefined styles for the collapsing toolbar and its children:

app/res/values/styles.xml

```xml
<!-- ... -->

<!-- Uncomment these: -->
<style name="Widget.Shrine.CollapsingToolbar" parent="">
    <item name="collapsedTitleTextAppearance">@style/TextAppearance.Shrine.Logo</item>
    <item name="contentScrim">?attr/colorPrimary</item>
    <item name="expandedTitleGravity">top</item>
    <item name="expandedTitleMarginStart">124dp</item>
    <item name="expandedTitleMarginTop">140dp</item>
    <item name="expandedTitleTextAppearance">@style/TextAppearance.Shrine.Logo</item>
    <item name="scrimVisibleHeightTrigger">140dp</item>
</style>

<style name="Widget.Shrine.CollapsingToolbarImage" parent="">
        <item name="android:layout_marginTop">48dp</item>
    <item name="android:layout_marginLeft">-124dp</item>
    <item name="android:adjustViewBounds">true</item>
</style>

<style name="Widget.Shrine.CollapsingToolbarContent" parent="">
    <item name="android:layout_marginTop">160dp</item>
    <item name="android:layout_marginLeft">124dp</item>
    <item name="android:layout_marginRight">16dp</item>
    <item name="android:layout_gravity">top</item>
    <item name="android:orientation">vertical</item>
</style>

<!-- ... -->
```

Run the app:

Now, there is a nice, large landing page header that slides away, condensing into just the logo in the app bar. However, we can make better use of this enlarged content area.

![Shrine App Screenshot 1](https://codelabs.developers.google.com/codelabs/mdc-android/img/5177c0983718e612.png) ![Shrine App Screenshot 2](https://codelabs.developers.google.com/codelabs/mdc-android/img/6ed63fb3cc8bf4a2.png)

***

### Add More Content to the Expanded App Bar
To make good use of this expanded App Bar real estate, include some larger, immersive content that will collapse and fade out as the user scrolls. `CollapsingToolbarLayout` has built-in support. To use it, add the content as children of the `CollapsingToolbarLayout` and specify their behavior as `layout_*` flags.

First, let's add a large photo that will collapse slightly slower than its container, making it seem as if it's further away from the user than the content in front of it. Add a Volley `NetworkImageView` as the first child of the `CollapsingToolbarLayout`:

app/res/layout/shr_main.xml

```xml
<!-- ... -->
<android.support.design.widget.CollapsingToolbarLayout
    android:id="@+id/collapsing_toolbar"
    style="@style/Widget.Shrine.CollapsingToolbar"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:layout_scrollFlags="scroll|exitUntilCollapsed|snap">


    <!-- Add this view -->
    <com.android.volley.toolbox.NetworkImageView
        android:id="@+id/app_bar_image"
        style="@style/Widget.Shrine.CollapsingToolbarImage"
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        app:layout_collapseMode="parallax"
        app:layout_collapseParallaxMultiplier="0.75"/>

    <android.support.v7.widget.Toolbar
        android:id="@+id/app_bar"
        android:layout_width="match_parent"
        android:layout_height="?attr/actionBarSize"
        app:layout_collapseMode="pin"/>

</android.support.design.widget.CollapsingToolbarLayout>
<!-- ... -->
```

`NetworkImageView` allows us to load images from a URL, but `CollapsingToolbarLayout` will work just as well with a stock `ImageView` class (or any other `View` type). The important piece is adding the `layout_collapseMode` and `layout_collapseParallaxMultiplier` attributes. A `layout_collapseMode` of `parallax` tells the `CollapsingToolbarLayout` that this view will collapse at a slower or faster rate (specified by `layout_collapseParallaxMultipler`) than its container.

You can also add the text content shown in the mocks. Add a `LinearLayout` with two `TextViews` (one for the title and one for the description) inside the `CollapsingToolbarLayout` as well:

app/res/layout/shr_main.xml


```xml
<!-- ... -->
<android.support.design.widget.CollapsingToolbarLayout
    android:id="@+id/collapsing_toolbar"
    style="@style/Widget.Shrine.CollapsingToolbar"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:layout_scrollFlags="scroll|exitUntilCollapsed|snap">

    <com.android.volley.toolbox.NetworkImageView
        .../>

    <!-- Add these views: -->
    <LinearLayout
        style="@style/Widget.Shrine.CollapsingToolbarContent"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_collapseMode="parallax"
        app:layout_collapseParallaxMultiplier="0.65">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/shr_product_category"
            android:textAppearance="@style/TextAppearance.AppCompat.Display2"/>

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:paddingTop="16dp"
            android:text="@string/shr_product_description"
            android:textAppearance="@style/TextAppearance.AppCompat.Subhead"/>

    </LinearLayout>

    <android.support.v7.widget.Toolbar
        .../>

</android.support.design.widget.CollapsingToolbarLayout>
<!-- ... -->
```

As a final step, give the new `NetworkImageView` some content. Add to `MainActivity`'s `onCreate` method to select a product for the header, and make a network request for the product's associated image URL (obtained from our application's built-in product data) to get the image data:

app/java/MainActivity.java

```java
@Override
protected void onCreate(@Nullable Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.shr_main);

    Toolbar appBar = (Toolbar) findViewById(R.id.app_bar);
    setSupportActionBar(appBar);

    ArrayList<ProductEntry> products = readProductsList();
    ImageRequester imageRequester = ImageRequester.getInstance(this);

    // Add this code:
    ProductEntry headerProduct = getHeaderProduct(products);
    NetworkImageView headerImage = (NetworkImageView) findViewById(R.id.app_bar_image);
    imageRequester.setImageFromUrl(headerImage, headerProduct.url);

    RecyclerView recyclerView = (RecyclerView) findViewById(R.id.product_list);
    recyclerView.setHasFixedSize(true);
    recyclerView.setLayoutManager(new LinearLayoutManager(this));
    adapter = new ProductAdapter(products, imageRequester);
    recyclerView.setAdapter(adapter);

}

// Add this code as well:
private ProductEntry getHeaderProduct(List<ProductEntry> products) {
    if (products.size() == 0) {
        throw new IllegalArgumentException("There must be at least one product");
    }

    for (int i = 0; i < products.size(); i++) {
        if ("Perfect Goldfish Bowl".equals(products.get(i).title)) {
            return products.get(i);
        }
    }
    return products.get(0);
}
```

Run the app:

The large header is filled with eye-catching content that disappears as the user scrolls up.

![Shrine App Screenshot 1](https://codelabs.developers.google.com/codelabs/mdc-android/img/f368bb3bfdb889ab.png) ![Shrine App Screenshot 2](https://codelabs.developers.google.com/codelabs/mdc-android/img/a1184df98b4b5baf.png)