# JavaScript Pure Functions

## Objectives

1. Explain what a "pure" function is
2. Practice writing functions without side effects
3. Practice identifying side effects

## Overview

In many ways, this lesson should be a standard introduction to pure functions:
students can be presented with, e.g., a cash register that keeps a running
total:

```javascript
let total = 0

function ringUp(cost) {
  total += cost
}
```

It should be easy to see (with a slightly more detailed example) how keeping a
total like this can lead to problems for the (imaginary) customers.

Then we can introduce them to the pure way of writing such a function:

```javascript
function calculateTotal(items) {
  return items.reduce((sum, i) => sum + i.cost, 0)
}
```

Pure functions are idempotent by definition — probably good to highlight what
that means, too.

## Resources

- [Eric Elliott: Pure Functions](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-pure-function-d1c076bec976#.idtnqshvn)
