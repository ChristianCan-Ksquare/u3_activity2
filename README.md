# Activity 2 - Closure

### Christian Alejandro Can Pérez

## What is a closure?

Closure means that an inner function always has access to the variables and parameters of it’s outer function, even after the outer function has returned. Is
the combination of a function bundled together (enclosed) with references to its surrounding state (lexical environment).

## Give me an example of closure.

```
function counter() {
  var count = 0;

  function addTwoToCounter() {
    count += 2;
    console.log(count);
  }

  return addTwoToCounter;
}

const addTwoToCounter = counter();

addTwoToCounter();

```

## What is ()() in code?

The double parenthesis is used to access a function that is nested inside another function, like in the next example.

```
function message() {
  var msg = "Hello World";

  function showMessage() {
    console.log(msg);
  }

  return showMessage;
}

message()();

```

## Move the variable after the closure (the function inside the function) and explain what happens.

```
function counter() {

  function addTwoToCounter() {
    count += 2;
    console.log(count);
  }
  var count = 0;
  return addTwoToCounter;
}

const addTwoToCounter = counter();

addTwoToCounter();

```

It still works because the function addTwoToCounter hasn’t been executed yet, so it has time to find the variable count and then return the function.

## Change var for let and explain why the logic is not affected.

```
function counter() {

  function addTwoToCounter() {
    count += 2;
    console.log(count);
  }
  let count = 0;
  return addTwoToCounter;
}

const addTwoToCounter = counter();

addTwoToCounter();

```

It still works because it's the same as the previous example, nothing has been executed yet so the function has time to find the variable and the variable has time to instantiate, and then return the function. It doesn’t matter if it’s let or var.

## Scope chain, an example of it, how many closures can we nest.

We can grow the chaining of functions as much as we want, we can also put variables on differentes levels of the function and each one works as expected in a closure, as you can see in the next example.

```
function counterOne() {
  let var1 = 1;
  function counterTwo() {
    let var2 = 2;

    function counterThree(var3) {
      console.log(var1, var2, var3);
    }
    return counterThree;
  }
  return counterTwo;
}

counterOne()()(3);
```

## They are conflicts between the closure and the global scope?

The closure and the global scope are two differents scopes and each one has his own variables. Also, there is no conflict between variables with the same name, because one variable is inside the function scope while the other it’s in the global scope, so they don’t interfere with each other. In the next example the variable count of the global scope keeps being 0 even if a variable with the same name is in the closure.

```
var count = 0;

function counter() {

  function addTwoToCounter() {
    var count = 0;
    count += 2;
    console.log(count);
  }
  return addTwoToCounter;
}

const addTwoToCounter = counter();

addTwoToCounter(); // 2
console.log(count); // 0

```

## Advantages of closures.

Among the advantages of using closures are:

- You can use variables and not worry about the variable name being repeated within another function.
- You can make variables private so they can’t be accessed from another function.
- High order components. Commonly used in React development.
- Function factory. You can create a factory to produce multiple functions from a single function.

## What is data hiding and encapsulation?

_Data hiding_ is making the variables or functions private using closures so they can’t be accessed. _Data encapsulation_ means that you can’t access the data or methods of a given function from outside of it (they are bind together, data and the function acting on the data).

## Give me an example of privacy with closures.

Closure is useful when we want a function to have _"private"_ variables, so then we can improve the privacy in our code. For example:

```
const bankAccount = (inBalance) => {
  let balance = inBalance;

  return {
    getBalance: function () {
      return balance;
    },
    deposit: function (amount) {
      balance += amount;
      return balance;
    },
    retire: function (amount) {
      balance -= amount;
      return balance;
    },
  };
};

const christianAccount = bankAccount(10000);
balance = 7000; // <-- This doesnt do anything
console.log(christianAccount.retire(4000)); // 6000

console.log(christianAccount.deposit(500)); // 6500

console.log(christianAccount.getBalance()); // 65000
```

In the previous example we created a function to manage a bank account and we assign a variable balance to it. Here, applying the concept of closure, we can make the variable balance private so we can’t modify it in any way, so the account balance is always safe even if someone tries to modify from the global scope.

## What happens if you create two counters with the same closure?

If we create two different counters, we create two different lexical environments that do not interfere with each other, so each counter is independent of the other.

```
function counter(){
  let count = 0;

  function addOneToCounter(){
    count++;
    console.log(count)
  }
}

const counter1 = counter();
const counter2 = counter();

counter1(); //1
counter1(); //2
counter2(); //1
counter2(); //2

```

## How can we add more functions, like a decrement counter? Give an example of it.

We can create a constructor from the function Counter and add the functions we want to create to it. Each function inside the function Counter is a different closure.

```
function Counter() {
  let counter = 0;

  this.incrementCounter = () => {
    counter++;
    console.log(counter);
  };

  this.decrementCounter = () => {
    counter--;
    console.log(counter);
  };

  this.multiplyCounter = (x) => {
    counter = x * counter;
    console.log(counter);
  };
}

const counter = new Counter();
counter.incrementCounter(); // 1
counter.incrementCounter(); // 2
counter.decrementCounter(); // 1
counter.multiplyCounter(3); // 3
```

## What are the disadvantages of closures?

The main disadvantage is the over-consumption of memory. If the closure is active, the variables can’t be garbage collected, so we can have memory leaks. Another disadvantage is that creating a function inside a function leads to duplicity in memory that can make the application slower.
