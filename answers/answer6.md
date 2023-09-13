# Write a piece of code for the following problem and analyze its complexity: Given an array of distinct integers and a target sum, find two numbers in the array that add up to the target sum.
Code complexity is a measure of the number of decisions available to a runtime. It is normally measured for one given function. In Javascript, the following keywords produce a decision point:
`if`, `while`, `for`, `case`, `default`, `continue`, `goto`, `&&`, `||`, `catch`, `? :` (the ternary operator) and `??`.
The usual method of defining code complexity is simply to count the number of times that each keyword appears in a method, and add 1.

For example, consider the following code:
```
function findNums(sum, arr) {
    for (let i = 0; i < arr.length; i++) {
        const num1 = arr[i];
        for (let j = 0; j < arr.length; j++) {
            if (j !== i) {
                const num2 = arr[j];
                if (num1 + num2 === sum) {
                    return [num1, num2];
                }
            }
        }
    }
    return null;
}
```
Given a desired sum and an array of integers, this method will find a set of integers whose sum is as desired. Counting the 'decision keywords', this method has a total complexity of 5: 2 `for`'s, 2 `if`'s, and the number 1.

Contrast that with this code, inspired by [a Stack Overflow article](https://stackoverflow.com/a/68097491/14632909):

```
function findNums(sum, arr) {
    for (let i = 0; i < arr.length; i++) {
        const num1 = arr[i];
        const num2I = arr.indexOf(sum - num1);
        if (num2I !== -1) {
            const num2 = arr[num2I];
            return [num1, num2];
        }
    }
    return null;
}
```

This code does mostly the same thing (ignoring the repetition of numbers), but has a complexity of 3: one `for` statement, one `if` statement and the number 1.

Note that less complexity does not imply more efficient performance. Although example 2 is less complex, it also relies on the `indexOf` operator, whose complexity is unknown and outside the scope of this method's complexity. Performance analysis must be approached as a separate topic, and reconciled with complexity.