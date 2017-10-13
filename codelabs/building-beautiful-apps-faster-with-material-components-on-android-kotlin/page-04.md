# 4. Add Bottom Navigation

Apps need a way to navigate between screens. The mocks use the bottom navigation pattern, which is good for apps with a small number of navigation destinations that are all of similar importance and prominence.
Material Components provides *BottomNavigationView* to implement this pattern. It functions much like a menu, and can be configured almost entirely via XML resources.

![Shrine App Image](https://codelabs.developers.google.com/codelabs/mdc-android-kotlin/img/88254bee646c4af8.png)

***

### Add the view
`BottomNavigationView` needs to go on top of all the previously added content. Add it as the last child in `MainActivity`'s `CoordinatorLayout`:

app/res/layout/shr_main.xml

```xml
<!-- ... -->
    </android.support.design.widget.AppBarLayout>

    <!-- Add this view -->
    <android.support.design.widget.BottomNavigationView
        android:id="@+id/bottom_navigation"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_gravity="bottom"
        android:background="#ffffff"
        android:theme="@style/ThemeOverlay.Shrine.BottomNavigation"
        app:menu="@menu/shr_bottom_navigation"/>

</android.support.design.widget.CoordinatorLayout>
```

The added `BottomNavigationView` element makes use of a theme overlay to change the tint color of the selected icons. Add the theme overlay definition to the main `styles.xml` file:

app/res/values/styles.xml

```xml
<!-- ... -->
    <style name="ThemeOverlay.Shrine.BottomNavigation" parent="">
        <item name="colorPrimary">?attr/colorAccent</item>
    </style>

</resources>
```   


> **Important:** Make sure the item has a name of `colorPrimary` and **not** `android:colorPrimary`. The former is used by the Android support libraries (and Material Components for Android) and supported on all the platforms they support, while the latter is part of the Android framework, and only works on Lollipop (API 21) and above.   


Theme overlays are a feature of Android that allow you to override specific theme attributes for a `View` and all of its children, while leaving the rest of the Activity or Application theme intact. This feature is often used for pieces of the UI that have a different color scheme than the rest of the app, such as app bars that have a dark color scheme that are displayed inside a light-themed application. The theme overlay applied to our BottomNavigationView will change the primary color to the accent color from our main theme (replacing the white color with a blue-grey one). Since `BottomNavigationView` uses the theme's current primary color for tinting the active navigation icon, this has the effect of making the active navigation icon a blue-grey color.

This view also makes use of a menu resource to specify which items, icons, and labels to display in the navigation. Add a new menu resource:

app/res/menu/shr_bottom_navigation.xml

```xml
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">
    <item
        android:id="@+id/category_clothing"
        android:enabled="true"
        android:title="@string/shr_category_clothing"
        android:icon="@drawable/ic_favorite_vd_theme_24"
        app:showAsAction="ifRoom"/>
    <item
        android:id="@+id/category_home"
        android:enabled="true"
        android:title="@string/shr_category_home"
        android:icon="@drawable/ic_shrine_vd_theme_24"
        app:showAsAction="ifRoom"/>
    <item
        android:id="@+id/category_popsicles"
        android:enabled="true"
        android:title="@string/shr_category_popsicles"
        android:icon="@drawable/ic_shopping_cart_vd_theme_24"
        app:showAsAction="ifRoom"/>
</menu>
```


### Make the magic happen
Users generally want their navigation buttons to take them somewhere. Attach some listeners to the view and handle those changes. `BottomNavigationView` provides two listeners for navigation events. `OnNavigationItemSelectedListener` lets you know when a new navigation item has been selected, so that you can change to the new category. `OnNavigationItemReselectedListener` lets you know when the currently selected navigation destination was reselected. A common way to handle this is to scroll to the top of the current category's items.

Since the app currently doesn't have a lot of possible destinations, simulate navigation by shuffling the product list. Add a method to shuffle the product list in `MainActivity`:

app/java/MainActivity.kt

```kotlin
private fun shuffleProducts() {
    val products = readProductsList()
    Collections.shuffle(products)
    adapter?.setProducts(products)
}
```

Attach the `OnNavigationItemSelectedListener` first in `MainActivity`'s onCreate. This will shuffle the list of products and scroll to the top:

app/java/MainActivity.kt

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    /* ... */

    bottom_navigation.setOnNavigationItemSelectedListener {
        val layoutManager = product_list.layoutManager as LinearLayoutManager
        layoutManager.scrollToPositionWithOffset(0, 0)
        shuffleProducts()
        true
    }
}
```


Below that method call (still inside of `MainActivity`'s onCreate), add the `OnNavigationItemReselectedListener`. This will scroll to the top of the list:

app/java/MainActivity.kt

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    /* ... */

    bottom_navigation.setOnNavigationItemReselectedListener {
        val layoutManager = product_list.layoutManager as LinearLayoutManager
        layoutManager.scrollToPositionWithOffset(0, 0)
    }
}
```

### Finishing Touches
Given the categories, it would best for users to start in the Home section. `BottomNavigationView` allows you to set the selected item programmatically. Initialize things to the right state in `MainActivity`'s onCreate. Add a call to `setSelectedItemId` whenever the activity is starting fresh:

app/java/MainActivity.kt

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    /* ... */

    if (savedInstanceState == null) {
        bottom_navigation.selectedItemId = R.id.category_home
    }
}
```

Run the app:

The bottom navigation is easy to reach on the screen and gives the user a smooth way to move around the app. By tapping on the currently selected navigation icon, the user can quickly scroll back to the top of the content — a great resource when reading long lists.

![Shrine App Screenshot](https://codelabs.developers.google.com/codelabs/mdc-android-kotlin/img/37e3fc6836d2eb72.png)

***
