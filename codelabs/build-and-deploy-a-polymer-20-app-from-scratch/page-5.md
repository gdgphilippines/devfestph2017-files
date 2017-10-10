## 5. Create the user interface with existing Web Components

In the following steps, you'll import and use interface elements from [WebComponents.org](https://www.webcomponents.org/) to define the visual appearance of your app.

### Add a toolbar

1. Open a new terminal window.
 
 Type `cd whose-flag` to get to your root project folder.

 Install the `app-layout` elements:

```bower
bower install app-layout --save
```

Installing the `app-layout` elements means that they are added to your app's Bower dependencies, and will be available for your app to use in its code.

2. With a text editor, open `bower.json` to see what the above command has done:

### bower.json
```json
...
"dependencies": {
   "polymer": "Polymer/polymer#^2.0.0",
   "app-layout": "^2.0.1"
 },
...
```

3. From `src/whose-flag-app`, open `whose-flag-app.html` in a text editor. The first line in this file is a link to another file:

### whose-flag-app.html
```html
<link rel="import" href="../../bower_components/polymer/polymer-element.html">
```

This is an HTML Import. It is like a library - an HTML file that contains elements and functions that you can use to build other elements. This line is here because you need the Polymer library in order to do Polymer things.

4. Add the following line under the existing HTML Import:

### whose-flag-app.html
```html
<link rel="import" href="../../bower_components/app-layout/app-layout.html">
```

This tells Polymer that you're going to use the `app-layout` elements. Note that you don't need to import `<app-toolbar>` itself; we're importing the whole collection of `app-layout` elements.

5. Locate the following text in `whose-flag-app.html`:

### whose-flag-app.html
```html
<h2>Hello [[prop1]]!</h2>
```
This is just placeholder text, and you can safely delete it. Replace it with this:

### whose-flag-app.html
```html
<app-header>
   <app-toolbar>
     <div main-title>Whose flag is this?</div>
   </app-toolbar>
 </app-header>
```

#### Points to note:
 
- Adding custom elements to your code is that easy! Simply install and import them. Once that's done, you can drop them onto your page with HTML tags, just like you would with a standard `<h1>` or `<input>` element.
- The `<app-toolbar>` element looks for a main-title div element in order to figure out how to format it.

If you want a preview of your app at any time, you can run `polymer serve --open` from the `whose-flag` root project folder, using a command line. You can also leave the Polymer development server running--just remember to refresh your browser to see your changes.

Your app should look like this:
![](https://codelabs.developers.google.com/codelabs/whose-flag/img/d9e8fa3cd24f4134.png)

In the following steps, we will add the rest of the user interface.

## Add an image

1. **Install** `<iron-image>` by typing `bower install iron-image --save` from the root `whose-flag` project folder.
2. **Import** `<iron-image>` by placing the following code after the existing import links in `whose-flag-app.html` (remember, `whose-flag-app.html` is in `src/whose-flag-app`).
```html
<link rel="import" href="../../bower_components/iron-image/iron-image.html">
```
3. Add the following code to `whose-flag-app.html`, after the `</app-header>` close tag:

### whose-flag-app.html
```html
<div id="flag-image-container">
  <iron-image 
    id="flag-image"
    preload fade src="data/svg/BR.svg">
  </iron-image>
</div>
```

`<iron-image>` makes it easy to get your images to render nicely. The `preload` attribute makes sure that the image does not render until the browser has fully loaded it. Until the image is fully loaded, the `preload` attribute uses the image's background color (defined in CSS--we'll get to that shortly) as a placeholder.

The `fade` attribute works with the preload attribute. When the browser has fully loaded the image, it renders the image with a nice fade effect.

At the moment, the image we point to (`data/svg/BR.svg`) doesn't yet exist - so the appearance of your app won't have changed since the previous step.

## Add buttons

Add three buttons to the simple app interface.

1. First, install the `<paper-button>` element:
```
bower install paper-button --save
```
2. In `whose-flag-app.html`, import the `<paper-button>` element:
### whose-flag-app.html
```html
<link rel="import" href="../../bower_components/paper-button/paper-button.html">
```
3. Insert the following code straight after the closing `</iron-image>` tag. We'll put these buttons inside the `<div id="flag-image-container"> ...</div>` block to make sure they align with the image.
```html
<div id="answer-button-container">
  <paper-button id="optionA" class="answer">Brazil</paper-button>
  <paper-button id="optionB" class="answer">Uruguay</paper-button>
</div>
<p>A message will go here, telling you if you got it right.</p>
<paper-button class="another" id="another">Another!</paper-button> 
```
Your app should look like this
![](https://codelabs.developers.google.com/codelabs/whose-flag/img/409ad2510551d37a.png)

At the moment, these buttons don't do anything. That's fine as you are just defining the interface of your app.

### Delete placeholder code
In the <script></script> block, locate the following code, and delete it:

### whose-flag-app.html
```css
prop1: {
  type: String,
  value: 'whose-flag-app'
}
```
This step won't change the appearance of your app.


### Set up data for the app
Your app tries to load an image called `data/svg/BR.svg`, so you should make sure it can find it--and the other images and data you'll be using soon. In the following steps, you will set up the data for the app.
1. In the root project folder, create a folder called `data/`.
2. Click the **Download app data** button below and save the file to your computer.

### [**`Download the App Data`**](https://github.com/katejeffreys-projects/data/raw/master/data.zip)

3. Unzip the file and place its contents in the `data/` folder you just created.

You should now have the following folder structure:

![](https://codelabs.developers.google.com/codelabs/whose-flag/img/b01698589d1ca1f2.png)

You should see the flag of Brazil when you refresh your browser or serve your app to view changes.

[Click here to see what your project should look like at the end of this step](https://github.com/katejeffreys-projects/whose-flag/tree/end-of-step-05).

[Here's a demo of the app at this stage](https://whose-flag-end-of-step-5.firebaseapp.com/).
