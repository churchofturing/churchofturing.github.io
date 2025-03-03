---
title: "Closures: An Overview"
date: 2024-02-12T11:30:03+00:00
# weight: 1
# aliases: ["/first"]
author: "Church of Turing"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "A brief introduction to closures."
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
# cover:
#     image: "<image path/url>" # image path/url
#     alt: "<alt text>" # alt text
#     caption: "<text>" # display caption under cover
#     relative: false # when using page bundles set this to true
#     hidden: true # only hide on current single page
editPost:
    URL: "https://github.com/churchofturing/churchofturing.github.io/tree/main/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
tags:
  - "js"
  - "functional programming"
  - "closure"
  
---


Closures are an interesting technique for encapsulating functionality and state. I'll be exploring what closures are, where they are useful and how they came about. The examples will be in JavaScript which is lexically scoped, but I'm keen to emphasise that lexical closures are a subset of closures in general.

## What are closures?

A closure can be thought of as a way of bundling a function with its *environment*. The key thing to stress here is that the combination of function and environment is what comprises the closure, and not just the function itself. 

To understand this a bit more intuitively, it's worthwhile to talk about open and closed lambda expressions. Take the following lambda for example:

```js
(n => n**2)
```

We could call this a **closed expression**, in that it's absent of any free variables. All the variables within the expression are bound, and to evaluate the function it's as straightforward as substituting `n`. 

```js
(n => n**2)(2) // (2 => 2**2) -> 4
(n => n**2)(8) // (8 => 8**2) -> 64
```

On the other hand, an **open expression** is one that includes one or more free variables - that is, a variable that is neither a parameter or local variable. Take the example:

```js
(n => n**2 + y)(2) // (2 => 2**2 + ???) -> ???
```

This is an open expression because we don't have enough information at this stage to be able to evaluate it - *where does y come from?* To be able to evaluate this expression we have to supply *y* from some surrounding environment.

We've already reached the heart of what a closure is: it's an open expression that is "closed" by supplying the environment. According to Joel Moses, Peter Landin introduced the term ["closure" to describe this situation](https://dspace.mit.edu/bitstream/handle/1721.1/5854/AIM-199.pdf?sequence=2&isAllowed=y).

## How is the environment supplied?

This depends on the language used and how it handles scoping. JavaScript for example is **lexically scoped**, meaning that the scope of the variable is defined by its textual location within the source code.

```js
const makeCounter = () => {
  let state = 0;
  return {
    show: () => state,
    increment: () => state++,
    decrement: () => state--,
  };
};  

let counter = makeCounter();
console.log(counter.show()) // 0
console.log(counter.increment()) // 1
console.log(counter.decrement()) // 0
```

The open expression `increment` finds the free variable "state" in the lexical environment, and this is what creates the closure.

Lexical closures are by far the most popular form of closures, to the extent that in casual conversation they're effectively synonymous. They are not however the *only* form of closures. Languages with other scoping mechanisms are capable of producing different types of closures. For example, Lisp 1.5 was dynamically scoped and could therefore leverage [dynamic closures](http://wiki.c2.com/?DynamicClosure).

## Where are they useful?

Closures are very versatile and can be used in many different situations, however I typically use them for:
- Event handlers
- State machines
- Iterators

### Event handlers

Closures are a nice way of encapsulating state for event handlers. Here's an example using client-side JavaScript to maintain a counter.

```js
// <button id="increment">+</button>
// <p id="value"></p>
// <button id="decrement">-</button>

function createCounter() {
  let counter = 0;
  return {
    currentState: () => counter,
    increment: () => ++counter,
    decrement: () => --counter,
  }
}

function onClick(elem, update, render) {
  elem.addEventListener('click', () => {
    update();
    render();
  });
}

const counter = createCounter();

const incrementBtn = document.getElementById("increment");
const decrementBtn = document.getElementById("decrement");
const value = document.getElementById("value");

// Display the default state
value.innerHTML = counter.currentState();
onClick(
  incrementBtn,
  counter.increment, 
  () => value.innerHTML = counter.currentState()
);

onClick(
  decrementBtn,
  counter.decrement, 
  () => value.innerHTML = counter.currentState()
);
```

### State machines

Here's an example of a state machine implemented using a closure. The state machine represents a simple turnstile, where it's locked until someone inserts a coin in which case they can push through it.

                   Insert
    +----------+    coin     +------------+
    |          +------------>|            |
    |          |             |            |
    |  Locked  |             |  Unlocked  |
    |          |             |            |
    |          |<------------+            |
    +----------+    Push     +------------+

This state machine can be represented using a closure like so:

```js
function createTurnstile() {
  let currentState = 'locked';
  const transitions = {
    'locked': {
      'insert-coin': 'unlocked',
    },
    'unlocked': {
      'push': 'locked'
    }
  };

  return {
    transition: (action) => {
      if (transitions[currentState][action]) {
        currentState = transitions[currentState][action];
        return currentState;
      } else {
        console.error(`Invalid ${action} for state ${currentState}`);
      }
    },
    currentState: () => currentState,
  };
}

const turnstile = createTurnstile();
console.log(turnstile.currentState()); // 'locked'
console.log(turnstile.transition('insert-coin')); // 'unlocked'
console.log(turnstile.transition('push')); // 'locked'
```

I've hardcoded the default state and the transitions within the function, but these can easily be passed as arguments to make a more generalised state machine creator. This is an exercise left to the reader; try it in your language of choice.

### Iterators

Closures can be used to make iterators, which are a useful abstraction for traversing lists.

Lets use an example of a (simplified) monopoly board.

```js
function createMonopoly(board) {
  let index = 0;
  return {
    currentState: () => board[index],
    step: (n = 1) => board[(index += n) % board.length]
  }
}

const monopoly = createMonopoly([
  "collect $200",
  "baltic avenue",
  "community chest",
  "boardwalk"
]);

console.log(monopoly.currentState()); // 'collect $200'
console.log(monopoly.step()); // 'baltic avenue
console.log(monopoly.step()); // 'community chest'
console.log(monopoly.step()); // 'boardwalk
console.log(monopoly.step(2)); // 'baltic avenue'
console.log(monopoly.step(3)) // 'collect $200'
```

This creates an iterator that cycles through the list, simulating the steps a player would take through a monopoly board.

### But...

Yes, your language of choice already *has* classes and iterators and so on. You might be asking "what's the point?". I don't have a compelling argument other than I think these abstractions are neat, elegant and fairly language independent (given that most mainstream languages support lexical closures at this point). Two of those reasons are entirely subjective and the other one isn't strong on its own. Use what you think is appropriate, but it never hurts knowing more than one way to do something. 

## Further reading

If you're interested in reading more, I would recommend:

- [The Wikipedia page on closures](https://en.wikipedia.org/wiki/Closure_(computer_programming))
- [c2 Wiki page on lexical closures](https://wiki.c2.com/?LexicalClosure)
- [MDN page on closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)
- [[PDF] A Tutorial Introduction to Lambda Calculus](https://personal.utdallas.edu/~gupta/courses/apl/lambda.pdf)
- [Let Over Lambda by Doug Hoyte - Chapter 2](https://letoverlambda.com/index.cl)