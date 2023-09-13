# Please follow the plunker link and provide a written *code review* for the sample code https://plnkr.co/edit/FZH2isUyd2n0JsWZ?open=lib%2Fscript.js

## Code
### index.html
```
<!doctype html>

<html>
  <head>
    <link rel="stylesheet" href="lib/style.css">
    <script src="http://code.jquery.com/jquery.min.js"></script>
    <script src="lib/script.js"></script>
  </head>

  <body>
    <h1 id="tag">Hello Plunker!</h1>
  </body>
</html>
```

### script.js
```
/** This should build a sentence and display it in the main browser window. */

var myStr = '';
first_part = 'I like to eat';

function GetSentence() {
  console.log('starting function');
  myStr = first_part;
  console.log('so far so good');
  for (var i = 0; i < 3; i++) {
    var x = ['apples', "bananas", 'cookies'];
    myStr = myStr + ' ' + x[i] + ' ';
    $('body')
      .find('h1')
      .text(myStr);
    console.log(myStr);
  }
}

GetSentence();
```

## Preliminary Statement
The only stated purpose of the code is defined in a comment in line 1 of `script.js`:
```
/** This should build a sentence and display it in the main browser window. */
```
Apparently this code has two goals: (1) to build a sentence, and (2) to display the sentence in the main window. Other behaviors are undefined; I list questions about the behavior for the developer below. In practice, I would strive to get answers to those questions before finalizing my comments.

## Questions
<ul>
<li>On lines 7, 9 and 14 of `script.js`, there are logs to the console. Usually production code does not log to the console. Should the logs be removed?</li>
<li>The jquery at lines 13-16 adds the text to the header, but due to when the script is run, the header shows some default text first. Is this behavior desired? Or should the header display the sentence only?</li>
<li>The sentence output is not grammatically correct. Is that okay?</li>
</ul>

## Comments
### Use of var
Lines 3, 10 and 11 use the `var` keyword to initialize variables. `var` is valid Javascript, but the scope of `var` is unpopular. ES6 introduced `let` and `const`: they are designed to have more clear behavior, and allow the underlying Javascript engine to run faster. I recommend using them. See [this article](https://hacks.mozilla.org/2015/07/es6-in-depth-let-and-const/).
### Use of redundant `first_part`
In line 4, `first_part` is initialized (without a keyword like `var` to define the initialization method):
```
first_part = 'I like to eat';
```
Then `myStr` is set to `first_part`, and `first_part` is not used again:
```
myStr = first_part;
```
I recommend initializing `myStr` to `first_part`'s value, and eliminating `first_part`.

### Use of redundant function `GetSentence`
`GetSentence` is defined in line 6, then immediately called in line 20. The developer may have some future purpose in mind of defining many functions, then calling them in a specific order later, but as it stands, there is no need for the function.

### Use of redundant `for` loop
In lines 10 to 16, a `for` loop is used to iterate through an array of strings. Each string in the array is added to the sentence. Arrays actually have a `.join` function which does the same work as what the array does now. I suggest using that instead.

### Redundant jquery call
Lines 13-15 update the header with the current text, but it does so within the `for` loop, updating the header with new text many times. This could be done once, after `myStr` is built completely, instead of within every iteration of the loop.

### Suggestions based on Commments
Here is sample code that incorporates all of the suggestions.
```
const x = ['apples', 'bananas', 'cookies'];
const myStr = `I like to eat ${x.join(' ')}`;
$('body').find('h1').text(myStr);
```
