### CART 263 / Winter 2019 / Pippin Barr

# jQuery

---

## Contents

- JavaScript and the DOM
- jQuery
- Selection and action
- Event handlers

---

## JavaScript and the DOM

- A major use of JavaScript in the context of the web is working with the DOM (Document Object Model)
- The DOM is a representation of the currently loaded webpage in terms of its elements and their content
- In JavaScript we have access to a variable called `document` which contains an object representing the DOM for the current page
- `document` also give us access to a number of functions for retrieving specific elements and manipulating them

---

## A webpage

```css
body {
  margin: 50px;
  font-family: sans-serif;
  font-size: 1.5em;
}

div {
  color: #4455aa;
  margin-top: 20px;
}

#greeting {
  color: #aabb00;
}
```

```html
<h1>Welcome to my homepage!</h1>
<div id="greeting">
  I'm so <span id="reaction">pleased</span> to see you!
</div>
<div>
  Want to know something? <a href="http://www.google.com">Google</a> it! Ha ha ha!
</div>
```

---

## Manipulating the page via the DOM

```javascript
"use strict";

window.onload = setup;

function setup() {
  let reaction = document.getElementById("reaction");
  reaction.innerText = "surprised";

  let divs = document.getElementsByTagName("div");
  for (let i = 0; i < divs.length; i++) {
    divs[i].style.color = '#ff0000';
  }

  reaction.addEventListener('click',reactionClicked);
}

function reactionClicked(e) {
  e.target.innerText = "depressed";
}
```

- So, we can access and manipulate elements on a webpage via "pure" JavaScript

???

- This is just a small example to show the basic premise of retrieving elements on a page and manipulating them in some way, as well as listening to events
- It does contain a bunch of the key elements here, notably:
  - Setting up code to run once the page has loaded
  - Accessing specific elements on the page (by id, by tag name)
  - Changing the content of elements on the page (via innerText)
  - Adding an event listener to an element (with addEventListener)
  - Writing event handler functions (like reactionClicked) that deal with the specific element targeted (via e.target)

---

## jQuery

- jQuery is an extremely popular JavaScript library created to make accessing, manipulating, and reacting to elements on a webpage easier, more efficient, and more compatible across browsers (among other things)
- Given its popularity, it's important to have at least a basic familiarity with its fundamental ideas
- This idea of using a webpage and its HTML elements as the output of creative programming is also very interesting and holds a lot of potential (as compared, for example, to working solely with an HTML canvas element) - we can make different kinds of experiences

---

## The jQuery library file

- As with any library, we should first go to its homepage and follow the instructions there
  - https://www.jquery.com
- Here we will see the option to download jQuery, but, if we read that download page more carefully we'll realise there are two options:
  1. We can download jQuery as a JavaScript file and include it in our project in the typical way
  1. We can use a CDN (Content Delivery Network) version of the library that is hosted online

???

- If we download the file, we force our users to download jQuery when they access our webpage, but we will be able to develop offline. If we use the CDN, we take advantage of caching, but if we don't have access to the internet, we can't work on our project
- Many of the very popular libraries around are available via a CDN

---

## The CDN option

- For now, we will use the CDN version by adding the following script tag to our project's `index.html`:

```html
<script
  src="https://code.jquery.com/jquery-3.3.1.min.js"
  integrity="sha256-FgpCb/KJQlLNfOu91ta32o/NMZxltwRo8QtmkMRdAu8="
  crossorigin="anonymous"></script>
```

---

## Document Ready

- A key concept when working with HTML/CSS via JavaScript is that we need to make sure the page has __loaded__ before we start trying to access elements on it
- One way to do this is to add our script tag at the __bottom__ of our `.html` file
- Another way is to rely on JavaScript that only runs when the document is ready to be manipulated, the jQuery version of this leads to programs written this way (in our `script.js`):

```javascript
$(document).ready(setup);

function setup() {
  // This code will run when the document is ready!
}
```

- This ensures our code will only run once the document is, well, ready.

---

## Anonymous functions

- In fact, a huge amount of the time in JavaScript when people are defining functions to be used as event handlers (like for "document ready") they use __anonymous functions__
- That is, they __define__ the function right in the place where you're supposed to put the event handler as a parameter:

```javascript
$(document).ready(function () {
  // This code will run when the document is ready
});
```

- This is one of those things that can be alarming when you're first encountering it
- But it's definitely something you will see __a lot__ online, so it's well worth getting used to

???

- And it's not __that__ bad
- Really we're just putting the definition of the function where previously we would have put the name of the function
- It means we've got one less step of indirection (going from the name to find the function code to run)
- At the expensive of somewhat weirder-looking syntax, with the function definition nested inside the parameters of another function
- You get used to it.

---

## Selection and action

- At the heart of programming with jQuery is the concept of __selection__
- Most jQuery code begins by __selecting__ one or more elements of a page and then applying some __action__ (i.e. a function) to them (even the "document ready" structure is __selecting__ the document object for instance)
- For example, if we want to fade in the `div` tags in our sample webpage we might write

```javascript
$(document).ready(setup);

function setup() {
  let $divs = $('div');
  $divs.hide();
  $divs.fadeIn(2000);
}
```

- Let's look at these more closely

???

- Note how you see a flicker of the `div` elements just when the page loads?
- That's because the page loads and renders for one frame before jQuery's `hide()` function is applied to the elements
- If we wanted them to be hidden from the beginning we should set `display: none;` in their CSS
---

## Selection and action

```javascript
let $divs = $('div');
```

- When we use jQuery we'll see the dollar sign a lot, it's the name of the main jQuery function used for selection
- So `$('div')` means "select all the `div` elements on the page"
- The result the function returns is a special __object__ created by jQuery
- We can store the result of this selection in a variable (note it is common practice to name variables that will store jQuery objects with a leading `$`)
- The resulting selection has lots of special functions we can call on it with dot notation...

---

## Selection and action

```javascript
let $divs = $('div');
$divs.hide();
```

- One of the special functions the jQuery object allows is `hide()`
- As you might expect, it hides the specified elements on the page (which we selected using the jQuery `$` function)
- In effect it manipulates their CSS styling to set `display: none`

---

## Selection and action

```javascript
let $divs = $('div');
$divs.hide();
$divs.fadeIn(2000);
```

- The `fadeIn()` function from jQuery does what you might expect
- It fades the specified elements in over the specified amount of time
- (The elements have to be hidden first or they can't be faded in of course!)

---

## Selection and action in one

- In fact, we don't have to put a selection into a variable first, we can act directly on the selection while selecting it

```javascript
$('div').hide();
$('div').fadeIn(2000);
```

- Since the selection __returns__ the jQuery object, we can use it right away!

---

## Chaining effects

- Also in fact, we can often __chain__ these functions together, because much of the time each jQuery action __returns__ the object that it just worked on, meaning you can add another action!

```javascript
$('div').hide().fadeIn(2000);
```

- You can even write them on separate lines if that looks clearer

```javascript
$('div')
  .hide()
  .fadeIn(2000);
```

- Whether you actually want to do this kind of thing is up to you of course

---

## What kinds of actions are there?

- There are many kinds of jQuery actions we can apply to elements on the page
- This is where you need to make friends with the documentation of the [jQuery API](http://api.jquery.com/)
- In fact it's a great idea to explore it and see what's there, rather than searching for something specific
- Of particular note are actions that work with CSS, attributes, and effects

---

## What kinds of selection are there?

- The most basic kind of selection in jQuery is to specify a CSS selector (things like `div`, `p`, `.myClass`, `#myId`, etc.)
- But jQuery does provide more sophisticated selection possibilities, which you can read about in the [Selector](http://api.jquery.com/category/selectors/) section of the API documentation
- For instance if you wanted to select every div on the page that __doesn't__ have a class of `spooky` you could write

```javascript
$('div').not('.spooky')
```

- or

```javascript
$('div:not(.spooky)')
```

---

## Event handlers

- Moving quickly along, the other major aspect of jQuery is handling events that occur on the page
- This includes reacting to user input like mouse movements and clicks, key presses, and so on
- As well as to other browser events
- We use the `on()` function in jQuery to handle specific events on specific elements

```javascript
$divs.on('click',divClicked);

function divClicked() {
  $(this).fadeOut();
}
```

- Note that `$(this)` used inside an event handler selects the element the event occurred on! (The one that was clicked, here.)

???

- Notice how the click event now works all __all__ the divs on the page
- Notice how we used `this` inside the event handler function to specify the __div that is clicked__ rather than all the divs
- If we wanted all the divs to fade out when __any__ div was clicked we could have used `$divs.fadeOut()` instead
- Again remember that we will often see this done with anonymous functions:

```javascript
$divs.on('click',function() {
  $(this).fadeOut();
});
```

---

## Event handlers

- Many of jQuery's functions perform some kind of action __over time__
- `fadeOut()` takes an amount of time to complete, for instance
- Most of these kinds of functions also allow us to specify a function to call when the effect is completed:

```javascript
$divs.on('click',divClicked);

function divClicked() {
  $(this).fadeOut(2000,fadeComplete);
}

function fadeComplete() {
  console.log("Fade out completed!");
}
```

---

## Again, but with nested anonymous functions

- Many of jQuery's functions perform some kind of action __over time__
- `fadeOut()` takes an amount of time to complete, for instance
- Most of these kinds of functions also allow us to specify a function to call when the effect is completed
- Often it's the final argument in the function (but read the documentation!):

```javascript
$divs.on('click',function () {
  $(this).fadeOut(2000,function () {
    console.log("Fade out completed!");
  });
});
```

---

## jQuery!

- Those are the fundamental aspects of jQuery
- We __select__ one or more elements on our page and __act__ on them
- Or we add __event handlers__ for specific kinds of events to elements and __react__ to them

---

## There's so much more

- jQuery can do more than hide and fade elements in and out
- And it can handle more diverse events than just mouse clicks
- It can do so much more it's pointless contemplating explaining it all here
- Rather, we need to be able to engage with the jQuery API (Application Programming Interface)

http://api.jquery.com/

- (As well as the many, many tutorials and resources online.)

---

# Fin.
