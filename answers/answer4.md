# See https://docs.google.com/document/d/1oYl1czN2KySsh47KrXOmenyxvEhzaWWljFAMb6T2OJU/edit?usp=sharing. What is the order of the items in the displayed list? Explain.

As written, no cars would show up in modern browsers like Chrome due to the issue mentioned in question 3 involving the `DomContentLoaded` event. Once that is fixed, the order would be ford, honda, toyota, vw, bmw, mercedes. To explain, I will show the function involved, and give an explanation.
Here is the function involved:

```
function showCars() {
 const cars = [];
 showItem('ford');
 fetch('germanCars.json')
    .then(
        (resp) => {
        showItem('toyota');
        return resp.json();
    })
   .then((data) => {
     data.forEach(c => showItem(c));
   });
 showItem('honda');
}
```

Generally speaking, Javascript calls synchronous code as the code is encountered by the runtime. Asynchronous code, such as those appearing in the `.then` functions above, only happen when the `Promise` returned by `fetch()` is resolved.

## Sequence of cars
### 'ford'
The display of 'ford' is triggered in line 3:
```
showItem('ford');
```
This happens as soon as the runtime encounters the code.
### 'honda'
The display of 'honda' is triggered by line 13:
```
showItem('honda');
```
The reason why it is the second string to display, even though it is the last to appear in the code, is because the contents of lines 4-10 are within a Javascript promise, meaning that they will wait for the `fetch()` function to resolve before executing.

### 'toyota'
This happens in line 6. It is the first thing programmed to happen after the `fetch()` function.

### 'vw', 'bmw', 'mercedes'
This happens in line 11, and is the last thing that happens after the `fetch()` function.

