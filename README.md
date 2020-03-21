# JavaScript Style Guide

1. [Case sensitivity](#Case-sensitivity)
1. [Whitespace and semicolons](#Whitespace-and-semicolons)
1. [Comments](#Comments)
1. [camelCase](#camelCase)
1. [Identifiers](#Identifiers)
1. [Reserved keywords](#Reserved-keywords)
1. [References](#References)
1. [Curly braces](#Curly-braces)
1. [Nesting levels](#Nesting-levels)
1. [Abbreviations](#Abbreviations)

## Case sensitivity
JavaScript is case sensitive. It is common to start the name of a constructor with a capitalised letter, and the name of a function or variable with a lower-case letter.

Example:
```
let a = 9;
console.log(a); // 9
console.log(A); // throws a ReferenceError: A is not defined
```
## Whitespace and semicolons
++Spaces++, ++tabs++ and ++newlines++ used outside of string constants are called ==whitespace==. Whitespace in JavaScript source can directly impact semantics. Because of a technique called "automatic semicolon insertion" (ASI), some statements that are well formed when a newline is parsed will be considered complete (as if a semicolon were inserted just prior to the newline). Some authorities advise supplying statement-terminating semicolons explicitly, because it may lessen unintended effects of the automatic semicolon insertion.

There are two issues: five tokens can either begin a statement or be the extension of a complete statement; and five restricted productions, where line breaks are not allowed in certain positions, potentially yielding incorrect parsing.

The five problematic tokens are the open parenthesis "(", open bracket "[", slash "/", plus "+", and minus "-". Of these, the open parenthesis is common in the immediately-invoked function expression pattern, and open bracket occurs sometimes, while others are quite rare. The example given in the spec is:
```
a = b + c
(d + e).foo()

// Treated as:
//  a = b + c(d + e).foo();
// Should be written as:
//  a = b + c;
//  (d + e).foo()
```
with the suggestion that the preceding statement be terminated with a semicolon.

The five restricted productions are ++return++, ++throw++, ++break++, ++continue++, and ++post-increment/decrement++. In all cases, inserting semicolons does not fix the problem, but makes the parsed syntax clear, making the error easier to detect. ++return++ and ++throw++ take an optional value, while ++break++ and ++continue++ take an optional label. In all cases, the advice is to keep the value or label on the same line as the statement. This most often shows up in the return statement, where one might return a large object literal, which might be accidentally placed starting on a new line. For post-increment/decrement, there is potential ambiguity with pre-increment/decrement, and again it is recommended to simply keep these on the same line.
```
return
a + b;

// Returns undefined. Treated as:
//   return;
//   a + b;
// Should be written as:
//   return a + b;
```
## Comments
Comment syntax is prety clear and its non-observance can lead to errors in the code and a violation of its structure.
```js
// a short, one-line comment

/* this is a long, multi-line comment
  about my script. May it one day
  be great. */

/* Comments /* may not be nested */ Syntax error */
```
## camelCase
If the name of a variable or function consists of more than one word, it should be written using camelCase. It is the practice of writing phrases is such that every word or abbreviation in the middle of a phrase begins with a capital letter, without spaces or punctuation marks.
```
let carColor = 'black';
function getNameUser() {
  return user.name;
}
```
## Identifiers
The name of a variable, function or property is known as an ==identifier== in JavaScript. Identifiers consist of letters and numbers, but they cannot include any symbol outside of $ and _, and cannot begin with a number.

Example: 
```js
let *prise = 7;
let !prise = 7;
let 1prise =  7; //bad names

let prise = 7;
let _prise = 7;
let $prise = 7; //good names
```
## Reserved keywords
Identifiers also must not consist of any reserved keywords. Keywords are words in the JavaScript language that have a built-in functionality, such as ++var++, ++if++, ++for++, and ++this++.

You would not, for example, be able to assign a value to a variable named var.
```
var var = "Some value";
```
Since JavaScript understands ++var++ to be a keyword, this will result in a syntax error.
For a complete reference, please view this [list of reserved keywords (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#Reserved_keywords_as_of_ECMAScript_2015).

## References
+ Use ++const++ for all of your references; avoid using var.
> ##### Why? This ensures that you can’t reassign your references, which can lead to bugs and difficult to comprehend code.
```js
// bad
var a = 1;
var b = 2;
// good
const a = 1;
const b = 2;
```
+ If you must reassign references, use ++let++ instead of var.
> ##### Why? let is block-scoped rather than function-scoped like var.
```js
// bad
var count = 1;
if (true) {
  count += 1;
}
// good, use the let.
let count = 1;
if (true) {
  count += 1;
}
```
+ Note that both ++let++ and ++const++ are block-scoped.
```js
// const and let only exist in the blocks they are defined in.
{
  let a = 1;
  const b = 1;
}
console.log(a); // ReferenceError
console.log(b); // ReferenceError
```
## Curly braces
In most JavaScript projects curly braces are written in “Egyptian” style with the opening brace on the same line as the corresponding keyword – not on a new line. There should also be a space before the opening bracket, like this:
```js
if (condition) {
  // do this
  // ...and that
  // ...and that
}
```
A single-line construct, such as if (condition) doSomething(), is an important edge case. Should we use braces at all?

Here are the annotated variants so you can judge their readability for yourself:

:( Beginners sometimes do that. Bad! Curly braces are not needed:
```js
if (n < 0) {alert(`Power ${n} is not supported`);}
```
:( Split to a separate line without braces. Never do that, easy to make an error when adding new lines:
```js
if (n < 0)
  alert(`Power ${n} is not supported`);
```
;) One line without braces – acceptable, if it’s short:
```js
if (n < 0) alert(`Power ${n} is not supported`);
```
:) The best variant:
```js
if (n < 0) {
  alert(`Power ${n} is not supported`);
}
```
For a very brief code, one line is allowed, e.g. if (cond) return null. But a code block (the last variant) is usually more readable

This is a basic rule that all programmers use. It should be followed, especially during teamwork.

## Nesting levels
Try to avoid nesting code too many levels deep, it makes code difficult to read.

For example, in the loop, it’s sometimes a good idea to use the continue directive to avoid extra nesting.

For example, instead of adding a nested ++if++ conditional like this:
```js
for (let i = 0; i < 10; i++) {
  if (cond) {
    ... // <- one more nesting level
  }
}
```
We can write:
```js
for (let i = 0; i < 10; i++) {
  if (!cond) continue;
  ...  // <- no extra nesting level
}
```
A similar thing can be done with if/else and return.

For example, two constructs below are identical.

#####
##### Option 1:
```js
function pow(x, n) {
  if (n < 0) {
    alert("Negative 'n' not supported");
  } else {
    let result = 1;
    for (let i = 0; i < n; i++) {
      result *= x;
    }
    return result;
  }
}
```
##### Option 2:
```js
function pow(x, n) {
  if (n < 0) {
    alert("Negative 'n' not supported");
    return;
  }
  let result = 1;
  for (let i = 0; i < n; i++) {
    result *= x;
  }
  return result;
}
```
The second one is more readable because the “special case” of n < 0 is handled early on. Once the check is done we can move on to the “main” code flow without the need for additional nesting.

## Abbreviations
Use abbreviated object properties, it helps to make code shorter and easier to read.
```js
const johnSmith = 'John Smith';
const obj = {
  johnSmith: johnSmith,
};//bad way

const johnSmith = 'John Smith';
const obj = {
  johnSmith,
};//good way
```