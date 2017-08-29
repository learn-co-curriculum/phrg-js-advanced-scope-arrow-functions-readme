JavaScript Arrow Functions
---

## Objectives

1. Practice writing arrow functions
2. Explain how arrow functions differ from named functions
3. Describe situations where arrow functions come in handy

## Function, function, function, function, function

You're familiar by now with the standard `function foo() { return 'bar' }` style of functions in JavaScript.
Well, there is another way to write functions in JavaScript called arrow functions:

``` javascript
let arrowFunction = () => {
  return 'Arrow functions are great!'
};
```

These are called [arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) in reference to the little `=>` that characterizes them.

Arrow functions are called just like regular functions.

``` javascript
arrowFunction() // 'Arrow functions are great!'
```

Let's piece together how they work.

``` javascript
let arrowFunction = () => {
  return 'Arrow functions are great!'
};
```

Just like a regular function you've seen before, the body of an arrow function is declared inside the `{ }` brackets.  The arguments to the function go in the parentheses before the arrow.  And an arrow goes in between the brackets and the parentheses.  

In a divergence from regular functions, if we omit the curly braces from around the function body, arrow functions give us implicit returns.  But this only works if we write our arrow functions without brackets.  

``` javascript
let square = (n) => n * n

square(3) // 9

let notSquare = (n) => { n * n }
notSquare(3)
// undefined

let backToSquared = (n) => { return n * n }
backToSquared(3)
// 9

```
## Anonymity's the Name of the Game

All arrow functions are anonymous. This is unlike regular functions, which take their names from their identifiers.

``` javascript
function iHaveAName() {}

iHaveAName.name // 'iHaveAName'
```

But arrow functions don't have identifiers, so they're always anonymous.

``` javascript
(() => {}).name // ''
```

Instead, we can set a pointer to an arrow function, or pass an arrow function through as an argument to another function:

```javascript
  let square = (n) => n * n
  // note that the function is anonymous, but we point the variable square to the anonymous arrow function

  [1, 2, 3].map((n) => n * n )
  // [1, 4, 9]
  // The lightweight nature of arrow functions makes them useful for callbacks
```

## Arrow Functions and This

As we saw earlier, when a function is invoked from another function, the `this` value goes to global.  Let's see this again.

```js

  let person = {
    firstName: 'bob',
    greet: function(){
      return function reallyGreet(){
        return `Hi, I'm ${this.firstName}`
      }
    }
  }
  person.greet()()
  // Hi, I'm undefined
  // Here, we use two parentheses to invoke the returned function from person.greet()
```

As you can see, `this` becomes global from the inner function because that inner function is not a method of `person`.  However, if we use an arrow function, the inner function retains the scope of the method it was declared in.  Let's see it.

```js

  let person = {
    firstName: 'bob',
    greet: function(){
      return () => {
        return `Hi, I'm ${this.firstName}`
      }
    }
  }
  person.greet()()
  // "Hi, I'm bob"
```

Let's see this same principle as it applies to callbacks.  Let's see the difference between passing a regular function and an arrow function to our `map` method.

```js

let person = {
  firstName: 'bob',
  greet: function(){
    return [1, 2, 3].map(() => { return this })
  }
}

person.greet()
// {firstName: "bob", greet: ƒ}
// {firstName: "bob", greet: ƒ}
// {firstName: "bob", greet: ƒ}

[1, 2, 3].map(() => this)
// window
// window
// window
```
Notice that in both cases, the arrow functions retain the context that they were defined in.  In the first case, the arrow function was declared in the `greet` method, where the `this` value equaled person.  So the `this` value of the arrow function also was that `person` object.  In the second case, the arrow function was not declared not via a function called, but simply when the code was run, in the global scope.  Therefore, because the `this` value of an arrow function is the context the function was defined in, the `this` value returned window.

### Summary

In the lesson above, we saw that arrow functions allow us to declare functions with minimal syntax.  We saw that if we do not declare the function with brackets, that we do not to provide an explicit return value to the function.  Finally, we saw that the `this` value of an arrow function equals the `this` value at the time the function is declared.  

## Resources

- [MDN: Arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

<p class='util--hide'>View <a href='https://learn.co/lessons/javascript-arrow-functions'>Arrow Functions</a> on Learn.co and start learning to code for free.</p>
