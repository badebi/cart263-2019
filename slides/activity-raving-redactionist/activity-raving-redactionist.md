### CART 263 / Winter 2019 / Pippin Barr

# Activity: Raving Redactionist

---

## Brief

An experience in which:

- We see a screen of text with some passages "redacted" (covered in black bars)
- Over time the redactions disappear, revealing the text beneath them
- The user is trying to keep the information secret, so they click the secret text to restore the redaction
- If all the redacted passages disappear, the user has failed!

---

## 1. Start a project

Download a template JavaScript (not p5) project and include jQuery by adding the appropriate script tag to `index.html`, e.g.

```html
<script
src="https://code.jquery.com/jquery-3.3.1.min.js"
integrity="sha256-FgpCb/KJQlLNfOu91ta32o/NMZxltwRo8QtmkMRdAu8="
crossorigin="anonymous"></script>
```

---

## 2. Create the basic HTML content

1. Create a number of paragraphs (inside `div` tags or `p` tags) in your HTML `body` containing placeholder text (I often use a [lorem ipsum generator](https://www.lipsum.com/)).
1. Add `span` tags with a `class` attribute of "redacted" around the parts of text you want to be redacted

You should now be able to view the page in your browser, but of course it won't be redacted yet because that class doesn't exist!

???

```html
<div>
  Lorem ipsum dolor sit amet, <span class="redacted">consectetur adipiscing elit</span>. Suspendisse magna nisl, feugiat a pulvinar eu, placerat consequat ante. Morbi nec sem in quam sodales luctus ullamcorper nec turpis. Duis porta tellus et quam rutrum, quis condimentum nunc viverra. Donec venenatis orci sit amet dolor pellentesque, <span class="redacted">a elementum orci pretium</span>. Donec ullamcorper ipsum neque. Nulla facilisi. <span class="redacted">Morbi fringilla tellus sed erat vestibulum, at vehicula ex aliquam</span>.
</div>

<div>
  <span class="redacted">Ut consectetur cursus elit sit amet dictum</span>. Sed dictum, urna id laoreet fringilla, felis ligula interdum leo, in sagittis neque turpis sed purus. Ut lacinia posuere nunc, <span class="redacted">non vestibulum enim</span>. Duis ac neque at sem posuere sagittis eget id metus. Praesent porttitor nisl et eros varius cursus. Vestibulum tincidunt bibendum elit, sed pharetra quam. <span class="redacted">Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae</span>; Curabitur ut nisi eget massa ullamcorper porta.
</div>

<div>
  Cras accumsan id <span class="redacted">mauris et sollicitudin</span>. Ut varius tellus felis, eu lacinia massa gravida vel. Sed blandit diam vitae ipsum feugiat euismod sit amet vel neque. Praesent at dapibus velit. In tempor venenatis elit, sed pharetra sem pretium in. Sed eu arcu euismod, mattis enim ut, congue quam. <span class="redacted">Cras tempor, justo ac pretium egestas</span>, orci orci sodales ipsum, <span class="redacted">id lobortis tortor turpis quis velit</span>. Curabitur rhoncus nulla orci, non bibendum sem euismod quis. Sed a metus et sem elementum congue sit amet sit amet dolor. <span class="redacted">Aliquam bibendum velit id justo gravida suscipit</span>. Ut eu euismod felis. Sed in mi quis nisl aliquam interdum. Sed sit amet aliquet mi. Suspendisse quis nunc risus. Nulla facilisi. Aenean condimentum eros augue, quis dictum mi vestibulum in.
</div>

```

---

## 3. Set up basic CSS styles

1. Style the `body` and `p` or `div` tags however you want to create margins, fonts, sizes, etc. that suit you
1. Create a `redacted` class that sets the background color to black and the foreground color to transparent (this is to avoid people seeing the text when they highlight it), also remove text decoration if you want to
1. Create a `revealed` class that sets the background color to transparent, and the foreground color to red (to induce panic!)

When you view the page now, the text in the redacted spans should be black!

???

```css
body {
  font-family: Courier, monospace;
  width: 800px;
  margin-left: 100px;
  margin-top: 100px;
}

div {
  margin-top: 1em;
}

.redacted {
  background-color: black;
  color: transparent;
  text-decoration: none;
}

.revealed {
  background-color: transparent;
  text-decoration: underline;
  color: red
}
```

---

## 4. Start the program

In `script.js` create the basic "document ready" code so that we can start writing our program in jQuery

???

```javascript
$(document).ready(setup);

function setup () {
  // Code will go in here
}
```

- Or with an anonymous function:

```javascript
$(document).ready(function () {
  // Code will go in here
});
```

---

## 5. Thinking in functions

We're going to need a function that will randomly change a span from redacted to revealed. This function will be called repeatedly over time on all the spans so that eventually they'll all be revealed. For now, let's define that function.

Define a function called `updateSpan` (or similar) that
1. Generates a random number (note that JavaScript we use `Math.random()` to generate a random number between `0` and `1`)
1. Check if the random number is less than a probability (e.g. `0.1` for 10% likelihood)
1. If it is, remove the `redacted` class from the current span (using `this` as the selector) __and__
1. Add the `revealed` class to the current span (using `this` as the selector)

???

```javascript
function updateSpan() {
  let r = Math.random();
  if (r < 0.1) {
    $(this).removeClass('redacted');
    $(this).addClass('revealed');
  }
}
```

---

## 6. One function up

Each time we update the page we need to call `updateSpan` on __all__ span tags, so we need a function to do this, which we can call something like `update`. Define this function and in it:
1. Use `each` to call `updateSpan` on all `span` tags

???

```javascript
function update() {
  $spans.each(updateSpan);
}
```

---

## 7. Time!

We want the redactions to randomly disappear __over time__, so we'll need a timing function. We'll use the `setInterval()` function to call a function repeatedly over time. This function takes two parameters:
- a _function_ that call each time the interval passes
- the _length in milliseconds_ of the interval

1. In the function called by "document ready" call `setInterval` with your `update` function and an appropriate interval time as arguments (500 milliseconds is probably reasonable)

Once this works, you should see the spans disappearing over time!

???

```javascript
function setup () {
  setInterval(update,500);
};
```

---

## 8. Redaction!

We want the user to be able to redact revealed passages by clicking on them, so we need a click event handler using jQuery's `on` function

1. Define a new function called something like `spanClicked` to handle clicks, in the function you should add the "redacted" class to the clicked span (with the `this` selector) and also remove the "revealed" class from the clicked span (`this` selector)
1. In the "document ready" function, add a click event to all spans ("span" selector) using `on()` that calls your `spanClicked` function

???

```javascript
function setup () {
  setInterval(update,500);
  $('span').on('click',spanClicked);
};

function spanClicked() {
  $(this).removeClass('revealed');
  $(this).addClass('redacted');
}s
```

---

## 10. Efficiency

If you're going to be using it repeatedly, it's often better to __store__ a jQuery selection in a variable to avoid looking it up with jQuery every time. We can do this with all the spans on the page.

1. Declare a `$spans` variable at the top of the program
1. At the start of the "document ready" function, store the selection of all "span" tags in the variable
1. Throughout the program, wherever you used `$('span')` you can now use `$spans`

---

## Done!

Nice job!

---

# Fin.
