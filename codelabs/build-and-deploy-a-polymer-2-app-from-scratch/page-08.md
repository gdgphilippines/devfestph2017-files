# 8. Load country data from a file
We'll add some code to load the country data from a file. First, have a look inside the `whose-flag/data/` folder that you downloaded earlier. This folder contains:
- A file called `countrycodes.json` which stores a list of country codes and the names of those countries.

Here's an extract so you can see how the data is laid out:

### countrycodes.json
```json
{ "countrycodes":[
{
 "code": "AF",
 "name": "Afghanistan"
},
{
 "code": "AL",
 "name": "Albania"
},
{
 "code": "DZ",
 "name": "Algeria"
},...}]}
```
- A folder called `svg/` which contains images of each country's flag. The filenames for each image file are the same as their country code. We'll use this to display names and flags for the countries in the game.

### Use `<iron-ajax>` to load countrycodes.json

Instead of hardcoding the country names and flag image filename, you'll take these values from countrycodes.json. Loading this file is simple--you can use a Polymer component called `<iron-ajax>`.

1. From the root project folder, install <iron-ajax>:
```
bower install iron-ajax --save
```
2. Import `<iron-ajax>` by adding the following line to the imports section in `whose-flag-app.html`:

### whose-flag-app.html
```html
<link rel="import" href="../../bower_components/iron-ajax/iron-ajax.html">
```
Now we can use `<iron-ajax>` to load the file.

2. Locate the following piece of code inside whose-flag-app.html:

### whose-flag-app.html
```html
<app-header>
     <app-toolbar>
       <div main-title>Whose flag is this?</div>
     </app-toolbar>
   </app-header>
   <div id="flag-image-container">
     <iron-image
       id="flag-image"
       preload fade src="data/svg/BR.svg">
     </iron-image>
```

3. Insert the following block of code between `</app-header>` and `<div id="flag-image-container">`:

### whose-flag-app.html
```html
<iron-ajax
  auto
  url="data/countrycodes.json"
  handle-as="json"
  on-response="_handleResponse"></iron-ajax>
```

Your code should look like this:
### whose-flag-app.html
```html
<app-header>
  <app-toolbar>
    <div main-title>Whose flag is this?</div>
  </app-toolbar>
</app-header>
<iron-ajax
  auto
  url="data/countrycodes.json"
  handle-as="json"
  on-response="_handleResponse"></iron-ajax>
<div id="flag-image-container">
  <iron-image
    id="flag-image"
      preload fade src="data/svg/BR.svg">
  </iron-image>
```

Let's look at what this code does:

- `auto` is a setting that makes `<iron-ajax>` load automatically when its URL or parameters change.
- `url` specifies where `<iron-ajax>` will load from.
- `handle-as` tells `<iron-ajax>` to treat the loaded file as json.
- `on-response` specifies the function to be called (`_handleResponse`) when `<iron-ajax>` receives a response - that is, when it has loaded the file. This type of function is known as an event listener.

### Create an event listener

We will add the event listener, `_handleResponse`, to the scripts inside the WhoseFlagApp class definition. Before we do so, we need to add a property that it will use.

1. Inside the `properties` function, add a property called `countryList`:

### whose-flag-app.html
```javascript
static get properties() {
  return {
    countryA: {
      type: String,
      value: "Brazil"
    },
    countryB: {
      type: String,
      value: "Uruguay"
    },
    outputMessage: {
      type: String,
      value: ""
    },
    correctAnswer: {
      type: String,
      value: "Brazil"
    },
    userAnswer: {
      type: String
    },
    countryList: {
      type: Object
    }
  };
}
```

Now, we'll create the `_handleResponse method`. This method will do three things:

Place the contents of the country data file into `countryList`.
Choose values for the options the user can select from.
Choose which of these options is correct.
Place the following code inside the class definition for `WhoseFlagApp`:

#### whose-flag-app.html
```javascript
_handleResponse(event) {
  this.countryList = event.detail.response.countrycodes;
  this.countryA = this.countryList[0];
  this.countryB = this.countryList[1];
  this.correctAnswer = this.countryA;
}
```
### Use the data loaded from countrycodes.json

The data inside `countrycodes.json` is made up of objects with key-value pairs. Each object has a code and a name. `this.countryA` and `this.countryB` each contain one of these objects. For example:

#### countrycodes.json
```json
{
 "code": "AF",
 "name": "Afghanistan"
}
```
You can reference the items within the objects. For example, `countryA.code` is the string `"AF"` and `countryA.name` is the string `"Afghanistan"`.

You will now change your code to use this data structure.

1. Find the following code:

#### whose-flag-app.html
```html
<div id="answer-button-container">
  <paper-button class="answer" id="optionA" on-click="_selectAnswer">[[countryA]]</paper-button>
  <paper-button class="answer" id="optionB" on-click="_selectAnswer">[[countryB]]</paper-button>
</div>
```

Replace it with the following code:

#### whose-flag-app.html
```html
<div id="answer-button-container">
  <paper-button class="answer" id="optionA" on-click="_selectAnswer">[[countryA.name]]</paper-button>
  <paper-button class="answer" id="optionB" on-click="_selectAnswer">[[countryB.name]]</paper-button>
</div>
```

Now that we have assigned values from the data file to the country options, we can delete the hard-coded values in the properties function.

2. Change the properties section of your code to look like this:
#### whose-flag-app.html
```javascript
static get properties() {
       return {
         countryA: {
           type: Object
         },
         countryB: {
           type: Object
         },
         outputMessage: {
           type: String,
           value: ""
         },
         correctAnswer: {
           type: Object
         },
         userAnswer: {
           type: String
         },
         countryList: {
           type: Object
         }
       };
```

Note: You will be changing any property that holds an item from `countrycodes.json` to be of type `Object` instead of type `String`. (`userAnswer` and `outputMessage` don't change type, by the way! `userAnswer` is the `textContent` of the button the user clicks, while `outputMessage` is the confirmation message telling the user whether they answered correctly. They are still `String`s.)

3. Since `correctAnswer` is now an `Object` with `name` and `code` fields, instead of a `String`, update `_selectAnswer` to accommodate this:

#### whose-flag-app.html
```javascript
 _selectAnswer(event) {
       let clickedButton = event.target;
       this.userAnswer = clickedButton.textContent;
       if (this.userAnswer == this.correctAnswer.name) {
         this.outputMessage = `${this.userAnswer} is correct!`;
       }
       else {
         this.outputMessage = `Nope! The correct answer is ${this.correctAnswer.name}!`;
       }
     }
```

4. Finally, replace the hard-coded filename with a reference to the data loaded from `countrycodes.json`. Find the following code:

#### whose-flag-app.html
```html
<iron-image
  id="flag-image"
  preload fade src="data/svg/BR.svg">
</iron-image>
```
Change it to the following code:

#### whose-flag-app.html
```html
<iron-image
     id="flag-image"
     preload fade src="data/svg/[[correctAnswer.code]].svg">
  </iron-image>
```

[Click here to see what your project should look like now](https://github.com/katejeffreys-projects/whose-flag/blob/end-of-step-08/src/whose-flag-app/whose-flag-app.html).

[Here's a demo of the app at this stage](https://whose-flag-end-of-step-8.firebaseapp.com/).

