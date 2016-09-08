# JavaScript Pure Functions

## Overview 

We'll define and write pure functions and also rewrite functions to be pure. 

## Objectives

1. Define the inputs and outputs of a  "pure function" 
2. Define and identify side effects
3. Write functions without side effects

## What's a pure function?

![Blue Sky](http://vignette1.wikia.nocookie.net/breakingbad/images/4/43/Season_2_promo_pic_4.jpg/revision/latest?cb=20120617212256)

_Only 99.1% pure. Amateurs._

A pure function is a function that:
 
- returns output solely based on its input (the parameters, if there are any);
- is free from any side effects.

For example:

```js
const cart = [];

function addToCart(item) {
  cart.push(item);
}
```

`addToCart()` is _not_ a pure function: the function modifies a variable outside of its own scope. The result wouldn't
always be the same — what if we accidentally change the `item` variable? It's not returning a value either - if we can
call a function and not worry about the return value, it is by definition impure.

Instead: consider this:

```js
const cart = [];

function addToCart(cart, item) {
  return cart.concat([item]);
}
```

Here, `addToCart()` _is_ a pure function — it returns a new cart with the item added to it, but it doesn't modify any
shared state. It simply takes in two values (the cart and the item) and returns a new value (the updated cart). You
might be wondering why we're not calling `cart.push(item)` instead — that's because it modifies the original array,
which means that our function is mutating variables outside of its own scope (the `cart` variable that is passed in
would be updated). Impurity, begone!

We can also slightly rewrite this to make use of the ES2015 spread operator and arrow functions:

```js
const cart = [];

const addToCart = (cart, item) => [...cart, item];
```

Side effects can be things like `console.log`, saving something in a database (in the case of Node.js), fetching some
remote data, mutating shared program state that is outside of the function, ... All of these will make your function
impure. Pure functions make us more confident — without fail, it will yield the same result as long as we pass in the
same values. Easy as that!

## Idempotency
Fancy word, right? You'll sometimes notice this word being thrown around. It might sound intimidating, but all it means
is that the function can basically be repeated many times and you'd still end up with the same result. For example, a
function that sets the text value of an input is idempotent - you can call it as many times as you want with the same
value, it will still set the value on the input, no matter how many times you run the function.

Pure functions are by definition idempotent since all they do is take input values and return an output value. If the
output values stay the same, so will the result. An idempotent function, however, is not always a pure function. An
idempotent function can still have side-effects, for example, removing an item in a database. The function can be called
several times, and the item will still be gone. 

## Impostors are afoot
Some functions might _appear_ to be pure, but after closer inspection, they're not! For example, let's say we have some
data on our favorite superheroes:

```js
let heroes = [
  { firstName: 'Tony', lastName: 'Stark', heroName: 'Iron Man' },
  { firstName: 'Steve', lastName: 'Rogers', heroName: 'Captain America' },
  { firstName: 'Barry', lastName: 'Allen', heroName: 'The Flash' },
];
```

Now, let's create a function that adds an `initials` property to our heroes:

```js
function addInitialsToHeroes(heroes) {
  heroes.forEach(hero => {
    hero.initials = hero.firstName.charAt(0) + hero.lastName.charAt(0);
  });
  
  return heroes;
}

let heroesWithInitials = addInitialsToHeroes(heroes);
```

The `return heroes;` part might make us feel good about ourselves — we're returning a value based on the `heroes` input,
right? We're forgetting something though: the original array _and_ the objects are being modified in the process! After
running the function, we can access the original array to see that its elements have been modified as well:

```js
console.log(heroes[0].initials); // prints 'TS'
```

Let's rewrite our function to be _pure_ instead, and using some ES2015 goodies:

```js
const addInitialsToHeroes = heroes => heroes.map(hero => Object.assign({}, hero, {
  initials: hero.firstName.charAt(0) + hero.lastName.charAt(0),
}));
```

In our rewritten function, we're _mapping_ the array and then returning a _new object_ in its place using
`Object.assign()`. Now our original array isn't modified:

```js
let heroesWithInitials = addInitialsToHeroes(heroes);

console.log(heroes[0].initials); // prints undefined
```


## Resources
- [Eric Elliott: Pure Functions](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-pure-function-d1c076bec976#.idtnqshvn)
