## 7. Set up data bindings

The text of your buttons is currently hard-coded. You need to change this so that you can display the button text from a data source. To do this, you will bind the text of your buttons to properties of your `whose-flag-app` element.

At the moment, `whose-flag-app` has no properties:

### whose-flag-app.html
```javascript
 static get properties() {
    return {
    };
  }
```
1. Add two properties to `whose-flag-app`:

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
      }
    };
  }
```
Their values remain hard-coded for now. Now, you can bind the button text to these properties.

2. Find the following lines in `whose-flag-app.html`:
### whose-flag-app.html
```html
<paper-button id="optionA">Brazil</paper-button>
<paper-button id="optionB">Uruguay</paper-button>
```

Replace the hard-coded button text with data bindings:
### whose-flag-app.html
```html
<paper-button id="optionA">[[countryA]]</paper-button>
<paper-button id="optionB">[[countryB]]</paper-button>
```
This syntax is called data binding. `[[countryA]]` will be replaced by the value of `this.countryA` from your element. [[countryB]] will be replaced by the value of `this.countryB`.

3. Create another property called `outputMessage`:

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
      }
    };
  }
```

4. Locate the following placeholder text:

### whose-flag-app.html
```html
<p>A message will go here, telling you if you got it right.</p>
```

Replace it with a data binding:

### whose-flag-app.html
```html
<p>[[outputMessage]]</p>
```

5. Add a property to hold the correct answer, and a property to hold the user's answer. In the `properties` function, after the definition of `outputMessage`, create two more properties:

### whose-flag-app.html
```javascript
correctAnswer: {
    type: String,
    value: "Brazil"
  }, 
  userAnswer: {
    type: String,
    value: "Brazil"
  }
```

Remember to add a comma between each property, and to make sure that there is no comma after the last property.

Now listen for, and handle, the event that is fired when the user selects an answer. When a user clicks on an HTML button, the button fires an event. A web developer can listen for that event and create a function that will run whenever that event is fired. This function is called an event listener.

6. Add the following function to the class definition of `WhoseFlagApp`:
### whose-flag-app.html
```javascript
_selectAnswer(event) {
    let clickedButton = event.target;
    this.userAnswer = clickedButton.textContent;
    if (this.userAnswer == this.correctAnswer) {
      this.outputMessage = `${this.userAnswer} is correct!`;
    }
    else {
      this.outputMessage = `Nope! The correct answer is ${this.correctAnswer} !`;
    }
  }
```
Here's where this function will sit inside your code:

```javascript
<script>
    ...
    class WhoseFlagApp extends Polymer.Element {
      static get is() { return 'whose-flag-app'; }
      static get properties() {
        return {
          ...
          }
        };
      }
      _selectAnswer(event) {
        ...
        }
      }
    }
    ...
</script>
```

Now, make sure this function gets called.

7. Add `on-click="_selectAnswer"` to the attributes of each answer button:

### whose-flag-app.html
```html
<paper-button id="optionA" class="answer" on-click="_selectAnswer">[[countryA]]</paper-button>
     <paper-button id="optionB" class="answer" on-click="_selectAnswer">[[countryB]]</paper-button>
```
Notes:
- The `on-click="_selectAnswer"` syntax is special to Polymer. Polymer lets you declaratively specify a function to call when a button is clicked.
- When the `_selectAnswer` function is called, it automatically receives the event that called it as a parameter. Hence, you can write `_selectAnswer(event)` and use the `event` parameter to gain access to the details of that event.
- We store the details of the event in a variable, like so: `var clickedButton = event.target;`
- We access the country name attached to the button by extracting its `textContent`, like so: `this.userAnswer = clickedButton.textContent;`

[Click here to see what your project code should look like now](https://github.com/katejeffreys-projects/whose-flag/blob/end-of-step-07/src/whose-flag-app/whose-flag-app.html).

[Here's a demo of the app at this stage](https://whose-flag-end-of-step-7.firebaseapp.com/).