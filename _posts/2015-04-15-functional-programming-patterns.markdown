---
layout: post
title:  "Commonly used Functional Programming Principles"
date:   2015-04-15 16:04
---

     
### Table of Contents

1. Introduction

2. Functions

2.1. Currying and partial application

2.2 Higher orderism

2.3. Composition

3. Looping and Iterating: Recursion

4. Types: Algebraic Data Types

5. A tad about functors, monoids and monads
     
6. Bibliography and further reading



#### 1. Introduction

I've been studying Haskell for the past few months so this is a compilation on some insights I have had while trying to understand this very dificult (but very well designed) language. Hope this helps someone out there. If you spot some mistakes, please let me know!

First let me say that the “patterns” I'm gonna talk about here are not “design patterns” as in OO Design Patterns, but guidelines and principles for a good design in functional programming. Almost every FP language has a feature that matches one of these principles, and thats why I'm making this compilation. I'm gonna try to be as clear as possible on my writing but don't worry if you dont understand every little bit of this article, this is a very difficult and mind breaking subject if you are coming from an imperative paradigm (like me).

#### 2. Functions

Let us start from the basics. Functional Programming strives for a very generic way of programming which enforces parameterizing stuff whenever you can (decoupling data from behaviour).
In pure functional programming languages everything is a function. So, obviously, every design principle requires functions.

What functions are NOT:
    Functions are NOT methods on a class.
    Functions are NOT subroutines nor blocks.

Functions are things. In purely functional languages, we talk about functions as in the sense of mathematical functions. They mean equality. So functions are equations, they “explain” what the code is all about.

Mathematical Operators are ALSO functions. How does that work? In Haskell and like every other functional languages you can move them to the beginning of the expression and use them as an prefix operator.
    
    (+) 5 6


#### 2.1 Currying and Partial Application

So, let us explain the code I wrote in the previous example:
I called the function (+) and passed it two parameters. Pretty obvious, right? No. That's not what I dit. Actually, in most functional programming languages functions can only receive ONE parameter.

Every function that has more than one parameter is a  “curried” function, it applies the function to the one parameter it receives and generates a partially applied function which will keep doing this to its right associate member until it finishes and then generates the final value.

Using the previous example:

    ((+) 5) 6

The (+) operator receives the number 5 as a parameter and returns a partially applied function, which will then be applied to 5 generating the result.

Partial application is very common when dealing with lists. I'll talk more about it later.

Also, function application is right-associative. It always “walks” to the right.


#### 2.2.2. Higher-Orderism

A Higher-Order function is a function that:

* Receives a function as a parameter
* Returns another function

Higher-order functions are pretty common nowadays due to languages like Javascript, C# and a lot of common imperative-like programming lanuages implementing them. It has proven itself a very useful computational model because it serves as a “map” from a function to another function. 

A known example of higher-orderism is the mapReduce technology created by Google. MapReduce is a technique for dealing with large data sets on a cluster-like environment.

MapReduce is mainly composed by two functions:

The map() function, which receives a predicate - a partially evaluated function like (+10) - and a data structure (such as a list or a file) and applies the predicate to the list returning a changed data structure.

Here is the type signature of map in Haskell:
    
	(a -> b) -> [a] -> [b]

The reduce() (sometimes called fold) function is a recursive function (We’ll talk more about it in the next section) which is kind of like map but changes the size of the data (folds it) into a single value. Like an accumulator would do.

#### 2.3. Function Composition

Basically, it is what is says. You can combine (compose) functions to make another functions. Now, this is a very powerful tool, since it provides a way for encapsulating implementations and reducing lots of lines of code.

The formal definition of function composition is:

	(f . g) h = f (g (h))

And its type signature is:

	(f -> h) -> (g -> h) -> (h -> h)    



So that f, g and h are functions and “.” is the operator that binds (glues) them together.

But what does the equation above mean? In a general way, it means that: whenever you compose two functions f -> f and g -> g together, and apply h to them, you’ll end up with a new function h -> h.


Lets see a more practical example:

evenSum :: Integral a => [a] -> a
evenSum = (foldl' (+) 0) . (filter even)

#### 3. Recursion

Pure functional programming languages can’t have side-effects which means they cannot alter the state of variables, so there’s no way you can do a apply the very known imperative pattern of iterating n times through an mount of code. So that’s why we use recursion!

What is recursion? In a general matter, it is a full computational model. Programming language-wise speaking, it is defined by two behaviors:

One or more edge cases (base cases)

An expression or routine that when evaluated reduces all other non-edge cases to the base cases.

So, recursion usually means a function that calls itself internally until it produces the desired value. Some people will say recursion is a bad pattern because it is slower than for-loop like iteration. This is not true nowadays, due to compiler-time optimization algorithms that will, of course, optimize the slower recursion codes into more perfomatic algorithms.

So, let us dive into some actual piece of code. This is one of the most famous practical use of recursion. The fibonacci algorithm:

The problem: The next number of the fibonacci sequence is the sum of the two previous numbers.

fib n = if n > 2 then fib (n - 1) + fib (n - 2) else 1

Here in a more descritive way using guards (which are kind of like if, then, else statements):

fib n | n == 1 = 1
       | n == 2 = 1
       | otherwise = fib (n - 1) + fib (n - 2)

Pretty straight forward. If n is equal to 1 or 2, n applied to fib returns 1. If not (otherwise/else), it calls itself on (n - 1) and (n - 2) until n is evaluated to one of the base cases. Then it ends the recursion.

#### 6. Types: Algebraic Data Types

As I've done in the section about functions, let us start by talking about what types are not. 

Types are NOT classes.

Types are these special kind of “things” that separate data from behaviour and are used to enforce Design by correctness. To explain types in a functional point of view, I’ll use the model of Algebraic Data Types or, as I will from now on abbreviate: ADT. There is a whole lot of languages that implement ADT, such as Haskell, F#, Miranda and OCaml.

The ADT model sees data types in a more mathematical point of view. To understand it you have to think about types as in valid domain/range, input/output values. It starts with primitives and create new types by composing (glueing) them together. This is also called a composite type.

- You can use product types and sum types


#### 7. Functors, monoids and monads

- Functors: mappable functions

- Monoids:

- Monads: Chaining sequences of operations (imperative way)
- Always returns a Monad
- Respects to the monoid rules

- The Rules of the Monoid

1. Closure: When combining two things of the same type you will always get another thing of the same type.
2. Associativity: when combining more than two things the order of the combination (pairwisely speaking) doesnt matter
3. Identity: There is a special kind of “thing” that when combined with another “thing” returns this
original “thing”.

A monad is a monoid in the category of the endofunctors.


#### 8. Bibliography and further reading

http://www.scs.stanford.edu/14sp-cs240h/projects/burke.pdf
http://learnyouahaskell.com/
http://book.realworldhaskell.org/


