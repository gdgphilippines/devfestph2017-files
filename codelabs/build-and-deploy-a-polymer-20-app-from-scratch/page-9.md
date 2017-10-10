## 9. Randomize the country options

In this step, we make the game dynamic and playable.

Here's the existing code for selecting the countries to be displayed:

### whose-flag-app.html
```javascript
_handleResponse(event) {
  this.countryList = event.detail.response.countrycodes;
  this.countryA = this.countryList[0];
  this.countryB = this.countryList[1];
  this.correctAnswer = this.countryA;
}
```

We'll write a function, `__getRandomCountry`, that selects a random country from the array of countries`(this.countryList)`. We'll use this function to randomize the options the user is presented with.

The JavaScript math function, `Math.random()`, will help with this. `Math.random` returns a random number between 0 and 1, for example, `0.30201092490461323` or `0.06303133640534542`.

To get a random integer that we can use as an array index, we can multiply the random number by the length of the array and extract the integer part using `Math.floor`:
```javascript
__getRandomCountry() {
  return Math.floor(Math.random() * (this.countryList.length));
}
```
1. Place the new function in `whose-flag-app.html`, inside the class definition for `WhoseFlagApp`:

### whose-flag-app.html
```html
<link rel="import" href="../../bower_components/polymer/polymer-element.html">
...

<dom-module id="whose-flag-app">
  <template>
    ...
  </template>
  
  <script>
    ...
    class WhoseFlagApp extends Polymer.Element {
      ...
      _handleResponse(event) {
        this.countryList = event.detail.response.countrycodes;
        this.countryA = this.countryList[0];
        this.countryB = this.countryList[1];
        this.correctAnswer = this.countryA;
      }
      __getRandomCountry() {
        return Math.floor(Math.random() * (this.countryList.length));
      }
    }
    window.customElements.define(WhoseFlagApp.is, WhoseFlagApp);
  </script>
</dom-module>
```

2. Use `__getRandomCountry`, instead of hard-coded integers, as an array index for `this.countryList`:

```javascript
this.countryA = this.countryList[this.__getRandomCountry()];
this.countryB = this.countryList[this.__getRandomCountry()];
```

Modify your code to make sure that `countryA` and `countryB` are always different from each other.

3. Enclose the code for assigning values to `this.countryA` and `this.countryB` in a while loop:

```javascript
_handleResponse(event) {
  this.countryList = event.detail.response.countrycodes;
  while (!this.countryA || !this.countryB || (this.countryA.code == this.countryB.code)){
    this.countryA = this.countryList[this.__getRandomCountry()];
    this.countryB = this.countryList[this.__getRandomCountry()];
  }
  this.correctAnswer = this.countryA;
}
```

Now we'll randomize which button's answer is correct.

Add the following code to the `__handleResponse function`, after `this.correctAnswer = this.countryA;`:
```javascript
let coin = (Math.floor(Math.random() * 2));
this.correctAnswer = coin == 1 ? this.countryA : this.countryB;
```

Now that your app is capable of generating random questions, give the user an option to play again!

Add the following function to the class definition for `WhoseFlagApp`:
```
_restart() {
  window.location.reload();
}
```
6. Hook up the `_restart` function to the **Another!** button:
```html
<paper-button class="another" id="another" on-click="_restart">Another!</paper-button>
```
[Click here to see what your project should look like now](https://github.com/katejeffreys-projects/whose-flag/blob/end-of-step-09/src/whose-flag-app/whose-flag-app.html).

[Here's a demo of the finished app!](https://whose-flag.firebaseapp.com/).