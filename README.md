## A Gentle Introduction to Functional JavaScript ##

_Why should I care about immutability?_ Transistors cannot get any smaller. Clock rates plateaued 12 years ago. Multi-core is currently the preferred strategy for improving speed. If your code is not thread-safe it cannot be distributed in parallel across multiple cores.

When you re-assign (mutate) a variable you have introduced state. If you have a bug you need to figure out the sequence of events that
lead to the failure. When you mutate variables you throwing away that sequence.

_How do avoid state?_ Any time you need to represent a state change you pass the previous value to a function that returns a new value.

Imagine a banking application. If you create an account object that holds a balance variable and mutates it every time you deposit or withdraw money you have no way to print out a monthly bank statement with out implementing that functionality. If you return a new account object with an updated balance you will accumulate a history of the exact sequence that lead up to the current balance.

check out "Time Travel Debug" from Redux DevTools
![redux](https://cdn-images-1.medium.com/max/1600/1*BTRxlHu8WuCF4Iep4R44lA.gif "Redux DevTools")
