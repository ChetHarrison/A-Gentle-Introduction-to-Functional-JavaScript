## A Gentle Introduction to Functional JavaScript ##

Functional Programming tends to intimidate people at first with the scary names it borrowed from Category Theory to describe interfaces known as Algebraic Structures. It's application in JavaScript is further obfuscated by many different implementations with out standards much the same way Promises developed pre A+ spec. The current de facto specification is called ["Fantasy-land"](https://github.com/fantasyland/fantasy-land) dates back to 2013. With out a spec people create different names for the same functions.

JavaScript was not written as a functional programming language. So it had to figure out how to adopt this paradigm from other languages. As it turns out JS, with first class, higher-order functions and now in ES2015 proper tail-call optimization, is well suited.

Interfaces are abstract by nature. The more abstract they are the more powerful they become because they allow us to solve range of seemingly unrelated problems with a surprisingly small set of patterns. Functional Programming is worth striving for, and that's the good news. You don't have to flip a switch and go functional. You can apply bits and peaces as you learn.

_Why should I care about immutability?_ Transistors cannot get any smaller. Clock rates plateaued 12 years ago. Multi-core is currently the preferred strategy for improving speed. If your code is not thread-safe it cannot be distributed in parallel across multiple cores. Shared mutable state is not thread-safe.

When you re-assign (mutate) a variable you have introduced state. If you have a bug you need to figure out the sequence of events that
lead to the failure. When you mutate variables you are throwing away that sequence.

_How do avoid state?_ Any time you need to represent a state change you pass the previous value to a function that returns a new value.

Imagine a banking application. If you create an account object that holds a balance variable and mutates it every time you deposit or withdraw money you have no way generating a monthly bank statement with out implementing that functionality. If you return a new account object with an updated balance you will accumulate a history of the exact sequence that lead up to the current balance.

Check out "Time Travel Debug" from Redux DevTools
![redux](https://cdn-images-1.medium.com/max/1600/1*BTRxlHu8WuCF4Iep4R44lA.gif "Redux DevTools")


### Glossary ###

_First-Class Functions_ Functions that can be assigned to a variable.

_High-Order Functions_ Functions that can be passed and return functions.

_Side Effects_ When a function mutates state outside it's scope.

_Purity_ A function with no side effect that maps every input to exactly one corresponding output. One of the many benefits of pure function, and a good to test for purity is that it can be cashed.

_Imperative vs. Declarative_ A for loop is a very specific solution to looping. We call this Imperative because we are explaining __how__ to do the loop. With Array.prototype.map we are declaring __what__ we want. If I discover a more optimal way to map I can refactor my map function with out breaking all the code that calls it. we call this Declarative. So if you want to know if you are writing declarative code ask your self if you can refactor it with out breaking the code that calls it.

_Arity_ The number of parameters a function takes.

_Partial Application_ A function with an incomplete set of parameters.

_Currying_ A curried function that is passed fewer parameters than it's arity will return a partially applied function that will delay execution until it has been passed all of the required parameters.

_Point Free_ A style where we strive to pipe data from the output of one function to the input of the next avoiding named parameters. It is more concise, easier to read.

_Composition_ Chaining functions together by piping the output of one function to the input of the next in the composition. Composing pure functions will respect the law of associativity. This will allow us to safely apply some optimizations to our code.

_Associativity_


### Part One Homework ###

The patterns we will cover in Part Two are built on some simple and familiar building blocks. Probably the most common pattern in computing is the concept of and _Item_ and a _Collection_. When you write a function that will transform some input to some output it should be operating on an _Item_ not a _Collection_ because we have a way of mapping over a _Collection_ and applying that function to each _Item_.

[This is an excellent tutorial](http://reactivex.io/learnrx/) written by Jafar Husian, tech lead at Netflix. It should take you a few hours and will give you skills you can apply immediately.
