# A Gentle Introduction to Functional JavaScript #

If anyone has questions you can tweet me at @ChetHarrison

Functional Programming tends to intimidate people with the academic terms borrowed from Category Theory to describe interfaces known as _"Algebraic Structures"_. JavaScript was not designed to be a functional language. It has adopted this paradigm from other languages. Fortunately JavaScript is well suited to the the task thanks to first-class, higher-order functions and proper tail-call optimization provided in ES2015. The adoption of functional programming in JavaScript was further obfuscated by many different pre-spec examples the same way Promises developed pre A+ spec. The current de facto specification, ["Fantasy-land"](https://github.com/fantasyland/fantasy-land), was proposed April, 2013 and appears to have found broad adoption.

The more abstract a concept is the harder it is to understand. Interfaces are abstract by nature. The more abstract they are the more powerful they become because they allow us to solve a wider range of seemingly unrelated problems with a surprisingly small set of patterns.

The good news is you don't have to flip a switch and go functional. You can apply bits of functional technique as you learn them. I have been slowly building up my functional play book over the past 3 years. As a result every refactor becomes a lot more enjoyable.

Ultimately we will see that a stateless app is pointless. But you need to judiciously select the points in your code where you allow mutation to occur. Most of the internal organs of your code should strive to be immutable, and consequently, extraordinarily testable. The more mutations you can eliminate the better off you are.

__Why should your care about immutability?__ Chip manufactures are at the atomic boundaries of transistor miniaturization. As a result clock rates have not increased since 2004.

Multi-core is currently the preferred strategy for improving speed. Routines with shared mutable state cannot be distributed in parallel across multiple cores because is not thread-safe. See [Robert Martin's talk](https://www.youtube.com/watch?v=7Zlp9rKHGD4) on this topic. Just note: an "assignment statement" is totally benign a "reassignment statement" is problematic.

When you reassign (mutate) a variable you are introducing state change to your application. To reproduce a complex bug you often need the sequence of computations that lead to the failure. When you mutate variables you are throwing away that sequence.

__How do you avoid state change?__ Any time you need to model a state change __you pass the previous value to a function that returns a new value.__ Don't mutate the old value just return a new one.

Imagine a banking application. If you create an account object that holds a balance variable and mutates it every time you deposit or withdraw money you have no way generating a monthly bank statement listing all of your activity without implementing that functionality. If you return a new account object with an updated balance you will accumulate a history of the exact sequence that lead up to the current balance. That can be pretty useful ...

Check out "Time Travel Debug" from Redux DevTools
![redux](https://cdn-images-1.medium.com/max/1600/1*BTRxlHu8WuCF4Iep4R44lA.gif "Redux DevTools")

----------------
### Glossary ###

__First-Class Functions__ Functions are valid values like primitives and objects.

__Higher-Order Functions__ Functions that can take and return functions.

__Side Effects__ When a function mutates state outside it's scope.

__Referential Transparency__ an input of `a` will always return an output of `b`. One of the many benefits of referential transparency is that it can be cached. Caching is good test of referential transparency.

__Purity__ A referentialy transparent function with no side effects that is passed all the resources it needs to do it's job.

__Imperative vs. Declarative__ A `for` loop is a very specific solution to looping. We call this _"imperative"_ because we are explaining _how_ to do the loop. With `Array.prototype.map` we are declaring _what_ we want. If I discover a more optimal way to map I can refactor my map function with out breaking all the code that calls it. We call this _"declarative"_. So if you want to know if you are writing declarative code ask your self if you can refactor it with out breaking the code that calls it.

__Arity__ The number of parameters a function takes.

__Partial Application__ A function with an incomplete set of parameters.

__Currying__ A curried function that is passed fewer parameters than it's arity will return a partially applied function that will delay execution until it has been passed all of the required parameters. [see this post by Hugh Jackson](https://web.archive.org/web/20140714014530/http://hughfdjackson.com/javascript/why-curry-helps)

[This is the example JSBin from the talk](https://jsbin.com/qufoka/edit?js,console). Note JSBin can be a bit fussy so if it does not run at first try a refresh.

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

// Note composition reads from left to right!
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

### Examples ###
// Simulating stateful behavior without mutations
`https://jsbin.com/laxoba/edit?js,console`


### Part One Homework ###

This will probably be a bit reminiscent "the Karate Kid" where Daniel has to paint the fence a thousand times before Mr. Miyagi will show him real Karate moves. But you will find, like Daniel, that after you are done painting fences you will already know karate. The patterns we will cover in Part Two are built on some simple and familiar building blocks.

Probably the most common pattern in computing is the concept of and _Item_ and a _Collection_. When you write a function that will transform some input to some output it should be operating on an _Item_ not a _Collection_ because we have a way of mapping over a _Collection_ and applying that function to each _Item_.

[This is an excellent tutorial](http://reactivex.io/learnrx/) written by Jafar Husain, tech lead at Netflix. It should take you a few hours and will give you skills you can apply immediately.

Explore some of the FP utility libraries out there:

* [ramda](http://ramdajs.com)
* [lodash/fp](https://github.com/lodash/lodash/wiki/FP-Guide)

For a quick playground I would pull those libs into your favorite JSBin or CodePen etc. and try out some composition. All of Ramda's functions are curried. Many of those sites let you select a library from a pre-populated list. All of them will let you edit the HTML pane. If you don't see the library you would like you can past the following HTML in the page and you will have a reference to Ramda via it's exported namespace of "R" and lowdash-fp's "_".

```
<script src="https://cdnjs.cloudflare.com/ajax/libs/ramda/0.21.0/ramda.min.js"></script>
<script src="https://cdn.jsdelivr.net/lodash/4.13.1/lodash.min.js"></script>
<script src="https://cdn.jsdelivr.net/lodash/4.13.1/lodash.fp.min.js"></script>
```

[Here is a sample bin that's all set up](https://ChetHarrison.jsbin.com/qizoru/edit?html,js,console)

Go ahead and "clone" it from the drop down menu in the upper left corner and you should be set up.

--------------------


# Part 2 #

__The Path to Building a Monad__

Here is the big reveal: __The Array is just a container for mutable values__. After completing Jafar's tutorial you should be pretty handy with some basic functions that operate on JS Arrays. You now know how to side step mutations by producing a new value / values in a new Array.

Because a __pure__ function has referential transparency it will always return a value. Because it is __first class__ we can think of it as a __lazy value__.

Before we dive into Functors and Monads I would like to point out two ways of working with these containers. We can add them to an object and call them like `myObj.doThis()`, or we can write a `doThis` function that takes an object parameter and operates on it in the same fashion. ie

```
const result = [].map( doThis );

const result = map = ( func, array ) => array.map( doThis );
```
Much of Ramda's power comes from the fact that many of the methods defined in the Fantasyland algebraic structures are implemented in Ramda and "dispatch" to the methods of one of the parameters it is called with _if_ it has a method of the same name. For example: the Ramda docs say that `R.concat` "Dispatches to the concat method of the first argument, if present." That means if I have:

```
class Monad {
	constructor( value ) {
		this.value = value;
	}
}

Monad.prototype.of = Monad.of = value => new Monad( value );

Monad.prototype.concat = Monad.concat = function( monad ) {
	return Monad.of( [ this.value, monad.value ] )
}

Monad.prototype.toString = function() {
	return `Monad( ${ this.value } )`;
}
```

and we call it like this:

```
const result1 = Monad.of( 'foo' ).concat( Monad.of( 'bar' ) );
```
it is equivalent to:

```
const result2 = R.concat( Monad.of( 'foo' ), Monad.of( 'bar' ) );

console.log( result1.toString() === result2.toString() ); // true
```

Will log this this `Monad( [ 'foo', 'bar' ] )` to the console.

You start with a collection of items, and some way of combining them two at a time. These are the `Monoid` rules. `Monads` are a subset of `Monoids` so they gain much of their composability from enforcing them.

__Rule 1 (Closure)__: there is an operator that will take 2 inputs of the same type and produce one output of the same type. Example:

`1 + 1 = 2`

This takes two Integers and produces one Integer.

__Rule 2 (Associativity)__: When combining more than two things, the pairwise combinations you choose don't matter. Example:

`( 1 + 2 ) + 3 === 6` left association

 `1 + ( 2 + 3 ) === 6` right association

__Rule 3 (Identity)__: The result of combining the "identity" element (known as the "empty" in fantasyland) with x is always x. Example:
"Under the `+` operator the set of integers has an `Identity` of `0`"

`0 + 1 = 1` left identity

`1 + 0 = 1` right identity

Armed with these rules you can perform the digital mastication of `reduce` because `reduce` requires a `concat` function and an `empty` value (aka an identity).

A `Functor` has `map` function. I takes a `Functor` and a function, that takes a value, transforms it, and puts it back in a NEW `Functor` of the same type. The `Functor` you know well is JS's `Array`.

`[ 1, 2 ].map( x => x + 1 ) // returns [ 2, 3 ]`

and just to drive the Ramda point home, this does the same thing

`R.map( ( x => x + 1 ), [ 2, 3 ] )`

An `Applicative Functor` (AP) has an `of` method that is a factory for producing `Applicative Functors` _(note this is called RETURN in Haskell)_ and an `ap` method. This allows you to put a function into it's value and "apply" it to _values_ in other `Applicative Functors`. Here is the _key_ thing. The function you "lift" into your AP must be _curried_. In FP we are always passing in _one_ data parameter. The only way to feed a function that has multiple parameters (aka arity > 1) is to feed it one param at a time. Don't eat your dinner in one bite. Take it down in small bytes.

A Monad implements these preceding concepts but adds a new method... `chain` (known as "bind" in Haskell). (Sorry I'm a parent) `map` is like changing a diaper. You are going to take a value (a baby) out of a `Functor` (a diaper) do your "transformation" (clean the baby) and put it back into a _new_ `Functor` (use a new diaper). `chain` is like you have a value (a teenager) that says "I want to change clothes". Clothes are Monads so they use `chain` to take off one outfit (`Monad`) and put on another (`Monad`).

__References__

* [Fantasyland Specification](https://github.com/fantasyland/fantasy-land)
* [Scott Wlaschin is ON IT!](https://vimeo.com/97344498)
* [Functors Applicatives and Monads in Pictures!](http://adit.io/posts/2013-04-17-functors,_applicatives,_and_monads_in_pictures.html)
* [Monads if you want to go DEEP](http://homepages.inf.ed.ac.uk/wadler/papers/marktoberdorf/baastad.pdf)
* [Combinators](https://gist.github.com/Avaq/1f0636ec5c8d6aed2e45)
* [How to build up a recursive function](http://bit.ly/1rk0Bhp)

