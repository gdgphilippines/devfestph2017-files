# Style your app

Let's give your app some style. You will use an existing color scheme found in the `<paper-styles>` component, which provides an easy implementation of material design.

1. Install `paper-styles`. Type the following command from the root `whose-flag` project folder:
```
bower install paper-styles --save
```
2. Import the color scheme inside `paper-styles` by adding the following code to the list of imports at the start of `whose-flag-app.html`:

### whose-flag-app.html
```html
<link rel="import" href="../../bower_components/paper-styles/color.html">
```

3. Replace the existing `<style></style>` block with the following code:
```css
<style>
      :host {
        display: block;
        font-family: Roboto, Noto, sans-serif;
      }
      paper-button {
        color: white;
      }
      paper-button.another {
        background: var(--paper-blue-500);
        width: 100%;
      }
      paper-button.another:hover {
        background: var(--paper-light-blue-500);
      }
      paper-button.answer {
        background: var(--paper-purple-500);
        flex-grow: 1;
      }
      paper-button.answer:hover {
        background: var(--paper-pink-500);
      }
      app-toolbar {
        background-color: var(--paper-blue-500);
        color: white;
        margin: 20px 0;
      }
      iron-image {
        border: solid;
        width: 100%;
        --iron-image-width: 100%;
         background-color: white;
      }
      #flag-image-container {
        max-width: 600px;
        width: 100%;
        margin: 0 auto;
      }
      #answer-button-container {
        display: flex; /* or inline-flex */
        flex-flow: row wrap;
        justify-content:space-around;
      }
```

- The CSS you put inside an element's `<template>` definition is scoped to that element. This means that it will only apply inside that element - the rest of the page won't know anything about it.
- However, you also want to be able to define a consistent style for the app itself. That's where the `:host` selector comes in. The `:host` selector allows you to style an element's parent. In this case, you use it to define a font choice for the whole app.
- We also use custom properties to define other style attributes. For example, we did this in the CSS code above:
```css
paper-button.another:hover {
  background: var(--paper-light-blue-500);
}
```
`--paper-light-blue-500` is a custom CSS property. You imported this custom CSS property with `paper-styles`, so it is now available to use.

The code extract above takes `--paper-light-blue-500` and assigns it to act as the value of the background color for `paper-buttons` of class `another` when they are being hovered over.

Remember that you can serve and test your app at any time by typing the following command from the root `whose-flag` project folder:
```
polymer serve --open
```

[Go here to see what your project code should look like now.](https://github.com/katejeffreys-projects/whose-flag/blob/end-of-step-06/src/whose-flag-app/whose-flag-app.html)

[Here's a demo of the app at this stage](https://whose-flag-end-of-step-6.firebaseapp.com/).