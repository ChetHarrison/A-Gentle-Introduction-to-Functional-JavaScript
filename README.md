# A Gentle Introduction to Functional JavaScript #

Functional Programming tends to intimidate people with the academic terms borrowed from Category Theory to describe interfaces known as _"Algebraic Structures"_. JavaScript was not designed to be a functional language. It has adopted this paradigm from other languages. Fortunately JavaScript is well suited to the the task thanks to first-class, higher-order functions and proper tail-call optimization provided in ES2015. The adoption of functional programming in JavaScript was further obfuscated by many different pre-spec examples the same way Promises developed pre A+ spec. The current de facto specification, ["Fantasy-land"](https://github.com/fantasyland/fantasy-land), was proposed April, 2013 and appears to have found broad adoption.

The more abstract a concept is the harder it is to understand. Interfaces are abstract by nature. The more abstract they are the more powerful they become because they allow us to solve a wider range of seemingly unrelated problems with a surprisingly small set of patterns.

The good news is you don't have to flip a switch and go functional. You can apply bits of functional technique as you learn them. I have been slowly building up my functional play book over the past 3 years. As a result every refactor becomes a lot more enjoyable.

Ultimately we will see that a stateless app is pointless. But you need to judiciously select the points in your code where you allow mutation to occur. Most of the internal organs of your code should strive to be immutable, and consequently, extraordinarily testable. The more mutations you can eliminate the better off you are.

__Why should your care about immutability?__ If you are going to send an electron down a conduit it has to be big enough for the electron. Chip manufactures are at the atomic boundaries of conduit miniaturization. As a result clock rates have not increased since 2004.

Multi-core is currently the preferred strategy for improving speed. Shared mutable state cannot be distributed in parallel across multiple cores because  is not thread-safe. See [Robert Martin's talk](https://www.youtube.com/watch?v=7Zlp9rKHGD4) on this topic. Just note: an "assignment statement" is totally benign a "reassignment statement" is problematic.

When you reassign (mutate) a variable you are introducing state change to your application. To reproduce a complex bug you often need the sequence of computations that lead to the failure. When you mutate variables you are throwing away that sequence.

__How do you avoid state change?__ Any time you need to model a state change __you pass the previous value to a function that returns a new value.__ Don't mutate the old value just return a new one.

Imagine a banking application. If you create an account object that holds a balance variable and mutates it every time you deposit or withdraw money you have no way generating a monthly bank statement listing all of your activity without implementing that functionality. If you return a new account object with an updated balance you will accumulate a history of the exact sequence that lead up to the current balance. That can be pretty useful ...

Check out "Time Travel Debug" from Redux DevTools
![redux](https://cdn-images-1.medium.com/max/1600/1*BTRxlHu8WuCF4Iep4R44lA.gif "Redux DevTools")

----------------
### Glossary ###

__First-Class Functions__ The ability to assign a function to a variable.

__High-Order Functions__ Functions that can take and return functions.

__Side Effects__ When a function mutates state outside it's scope.

__Purity__ A function with no side effects that maps every input to exactly one corresponding output. One of the many benefits of a pure function is that it can be cashed. Cashing is good but not complete test of purity but side effects are pretty easy to spot.

__Imperative vs. Declarative__ A `for` loop is a very specific solution to looping. We call this _"imperative"_ because we are explaining _how_ to do the loop. With `Array.prototype.map` we are declaring _what_ we want. If I discover a more optimal way to map I can refactor my map function with out breaking all the code that calls it. We call this _"declarative"_. So if you want to know if you are writing declarative code ask your self if you can refactor it with out breaking the code that calls it.

__Arity__ The number of parameters a function takes.

__Partial Application__ A function with an incomplete set of parameters.

__Currying__ A curried function that is passed fewer parameters than it's arity will return a partially applied function that will delay execution until it has been passed all of the required parameters. [see this post by Hugh Jackson](https://web.archive.org/web/20140714014530/http://hughfdjackson.com/javascript/why-curry-helps)

```
const add = ( a, b ) => a + b; // has an arity of 2
const addC = curry( add ); // returns a curried add function
add3 = addC( 3 ); // add was partially applied

// the addC( 3 ) call will return a new function that looks like this
b => 3 + b;
console.log( add3( 1 ) ); // logs 4
```

__Point Free__ A style where we strive to pipe data from the output of one function to the input of the next avoiding named parameters. It is more concise, easier to read, and lends itself to composition.
```
const inc = x => x + 1; // x is a point
const add3 = compose( inc, inc, inc ); // no points here
const val = add3( 1 ); // val === 4

// compose will return something like this
x => inc( inc( inc( x ) ) );
```

__Composition__ Chaining functions together by piping the output of one function to the input of the next in the composition. Composing pure functions will respect the law of associativity. This will allow us to safely apply some optimizations to our code.

__Associativity__ A binary operation in which the order of evaluation is not important. Addition of whole numbers is associative.

```
const a = ( 1 + 2 ) + 3; // 6
const b = 1 + ( 2 + 3 ); // 6
a === b; // true
```

Subtraction of whole numbers is not associative.

```
const a = ( 1 - 2 ) - 3; // -4
const b = 1 - ( 2 - 3 ); // 2
a === b: // false
```

__Container__ an object that you can place a value in with a function that takes a function and applies it to the value as input and returning a new container of the same type with the result as output.


### Part One Homework ###

This will probably be a bit reminiscent "the Karate Kid" where Daniel has to paint the fence a thousand times before Mr. Miyagi will show him real Karate moves. But you will find, like Daniel, that after you are done painting fences you will already know karate. The patterns we will cover in Part Two are built on some simple and familiar building blocks.

Probably the most common pattern in computing is the concept of and _Item_ and a _Collection_. When you write a function that will transform some input to some output it should be operating on an _Item_ not a _Collection_ because we have a way of mapping over a _Collection_ and applying that function to each _Item_.

[This is an excellent tutorial](http://reactivex.io/learnrx/) written by Jafar Husain, tech lead at Netflix. It should take you a few hours and will give you skills you can apply immediately.

Explore some of the FP utility libraries out there:

* [ramda](http://ramdajs.com)
* [lodash/fp](https://github.com/lodash/lodash/wiki/FP-Guide)

For a quick playground I would pull those libs into your favorite JSBin or CodePen etc. and try out some composition. All of Ramda's functions are curried.
