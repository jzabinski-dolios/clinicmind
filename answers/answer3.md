# See https://docs.google.com/document/d/1q-5lE5jEvMHj95SfhQlJzk_7q3MpQbzOGQ7IsXQUAec/edit?usp=sharing. What is wrong with the function `computeTotal()`?

The root cause of the issue in `computeTotal` is that jquery's `.each` function is being called with an arrow function:
```
$("li").each(() => {
   total += parseInt($(this).text(), 10);
 });
```

Normally, the `this` in `$(this)` refers to the jquery element being iterated upon by `.each`. The arrow function disrupts this functionality, repointing `this` to the scope immediately above the function. Since this is being done by a global script, the scope immediately above is the `Window` object. That has no `.text` function, so it returns `undefined`.

To resolve the issue, there are a few alternatives. One is to revert back to the older `function` terminology for which jquery is designed:
```
$("li").each(function() {
   total += parseInt($(this).text(), 10);
 });
```

jquery will then act as expected.

However, the scope of `this` is often confusing and difficult to troubleshoot. I recommend avoiding `$(this)` if possible when writing code for modern browsers. jquery provides alternate access to DOM elements, which would allow for this functionality to use modern function syntax:

```
$("li").each((unused, element) => {
   total += parseInt(element.innerHTML, 10);
 });
```

Although it was not asked for, I want to mention that `computeTotal` might not ever be called as written. It is being called through an event listener listening to the `DOMContentLoaded` event, which will have already been triggered by the time that this script is run. Running the contents of the function directly in the script would allow the `li` elements to be successfully updated.

Changing the contents of `script.js` to the following would ensure that the code runs:

```
let total = 0;
$("li").each((index, element) {
    total += parseInt(element.innerHTML, 10);
});
$("#total").text(`Total: ${total}`);
```