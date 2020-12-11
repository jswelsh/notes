
[**source!!!!!**](https://gist.githubusercontent.com/ericelliott/414be9be82128443f6df/raw/94c8be8bdfd2d741b834dc56906b017f4231b187/lambda-not-anon.md)

#Lambda means "function expression used as data".

Anonymous function means "function without a name".

This is one of the relatively few cases where the Wikipedia definition of a word, while not entirely wrong, is misleading. Lambdas and anonymous functions are distinct ideas.

These ideas are commonly confused because in many programming languages (and lambda calculus) all lambdas are anonymous or vise verse.

In JavaScript, not all lambdas are anonymous, and not all anonymous functions are used as lambdas, so the distinction has some practical meaning.


```js
(function () {
  console.log(`
  Some people mistakenly think that "lambda" and "anonymous function" have
  the same meaning. Let's clear that up.

  In computer science, the most important, defining characteristic of a lambda
  expression is that it is used as data. That is, the function is passed as
  an argument to another function, returned as a value from a function, or
  assigned to variables or data structures.

  This is basically the definition of a first class function. In JavaScript, you
  can use any function as a lambda expression because functions are first class.

  However, that does not mean that all JavaScript functions are therefore lambda
  expressions. For instance, this expression defines a function which gets
  immediately invoked and then dropped on the floor rather than passed,
  exported, or assigned. In other words, the function is not used as data.
  Instead, it's used for a side-effect (logging this text to the console).

  Conceptually, this is more akin to imperative programming than functional
  programming, and thinking of this function as a lambda would add zero useful
  meaning to your understanding of it.

  This distinction is not essential, but is a useful concept when you're
  learning functional programming. In other words, if you understand this
  distinction, you have a deeper understanding of what the word "lambda" means
  in both functional programming and lambda calculus (which are closely
  related).

  It is possible to use functional language features like first-class functions
  in an imperative style, but that adds nothing interesting to your grasp of the
  language, or your grasp of why lambda expressions exist in the first place.
  `)
})();

$('#el').on('click', function clickHandler () {
  console.log(`
  This is an example of a lambda expression that is not anonymous. As you can
  see, it clearly has a name, clickHandler, which can be used inside the
  function for the purpose of recursion (also an important concept in functional
  programming).

  It is a lambda expression because of the semantic use -- it's being passed
  to another function as data. The .on() function is using the function as
  an argument -- in other words, communicated as a message (i.e. functions as
  data).
  `);
});
```

---


Looks like some people are still confused, because **you've accepted "lambda = anonymous" as gospel**. While it is a somewhat useful concept to understand that anonymous functions have interesting uses, such as points-free-style (tacit programming), that is not the salient point of lambdas. It's mostly syntactical sugar.

I am aware that hundreds of references refer to lambdas and anonymous functions interchangeably, and that lots of references list "anonymous function" as the definition of "lambda".


**I'm saying they're wrong**, and the distinction matters in JavaScript in order to avoid confusion when you learn functional programming concepts.

**It is functions used as data which is the true salient, defining feature of lambdas, not anonymity.**

For example, you can have all the same benefits of anonymous functions even when those functions have names, simply by ignoring the names.

However, there are benefits to named lambdas that you can't enjoy without the names, such as enhanced debugging visibility.

The point here is that whether a function has a name or not is not particularly important compared to whether or not the function is used as data. JavaScript  supports named and unnamed lambdas, and it's useful to understand that a named function can be anonymous. If you don't consider a named function to be a lambda, you're missing the point of lambdas.

`bar` is a lambda, but not anonymous:

```js
const foo = [1,2,3];
const baz = foo.map(function bar (n) { return n + 1; });
```

This anonymous function is not a lambda:

```js
// Just evaluated and dropped on the floor. Not used as data.

// Not a lambda.

(msg) => {

  const formattedMsg = JSON.stringify({
    time: Date.now(),
    msg
  });
  console.log(formattedMsg)
  
}('foo');
```
`