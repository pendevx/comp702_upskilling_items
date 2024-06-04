# JavaScript Promises, async/await

## Promises

Promises are a new API introduced in ES6, to allow to write asynchronous code.

A promise may be in only one of two states:
- Pending
- Settled

A pending promise is one which has not completed yet. The promise is "awaiting" its result.

A promise is considered to be "settled" when it's no longer in a pending state. A settled promise is one which is either fulfilled, or rejected.

The Promises object also contains methods for chaining promises, including `.then()`, `.catch()`, and `.finally()`. These callbacks specifies code to run, which depends on the result of the promise it's chained onto.

- `.then()` runs if the previous promise was fulfilled,
- `.catch()` runs if the previous promise was rejected,
- `.finally()` runs when the previous promise was settled.

However, this new API still does not resolve the callback hell problem, it simply provides a newer and more procedural syntax to asynchronous tasks.

The Promises API also provides a few static methods on the `Promise` class to create an advanced promise based on the state of other promises, or collections of other promises:
- `Promise.all()`: A promise which fulfills when all input promises fulfill. Rejects with the reason of the first rejection.
- `Promise.allSettled()`: A promise which fulfills when all the input promises settle. 
- `Promise.any()`: A promise which fulfills when when any of the input promises fulfill.
- `Promise.resolve()`: An already-fulfilled promise with the argument as its fulfill value.
- `Promise.reject()`: An already-rejected promise with the argument as its rejection reason.

## async/await keywords

In ES7, two new keywords `async` and `await` were introduced, specifically to resolve the issue of Callback Hell.

`async/await` is syntactic sugar over the Promises API, which allows using the `await` keyword as syntactic sugar over the `.then()` method, and using `try-catch(-finally)` blocks as syntactic sugar for the `.catch()` and `.finally()` methods.

`async`hronous functions always return a `Promise`.

The below is an example of transforming a plain Promises chain into one using the `async`/`await` keywords:

```js
var a;
var b = new Promise((resolve, reject) => {
    console.log("promise1");

    setTimeout(() => {
        resolve();
    }, 1000);
}).then(() => {
    console.log("promise2");
}).then(() => {
    console.log("promise3");
}).then(() => {
    console.log("promise4");
});

a = new Promise((resolve) => {
    return new Promise((res) => {
        console.log(a);
        b.then(() => res());
    }).then(() => {
        return new Promise((res) => {
            console.log(a);
            console.log("after1");
            a.then(() => res());
        });
    }).then(() => {
        resolve(true);
        console.log("after2");
    })
});

console.log("end");
```

The code above shows an example of a purely Promises code. The below is the simplified version of it using `async` and `await`.

```js
var a;
var b = new Promise((resolve, reject) => {
    console.log("promise1");

    setTimeout(() => {
        resolve();
    }, 1000);
}).then(() => {
    console.log("promise2");
}).then(() => {
    console.log("promise3");
}).then(() => {
    console.log("promise4");
});

a = new Promise(async (resolve, reject) => {
    console.log(a);
    await b;
    console.log(a);
    console.log("after1");
    await a;
    resolve(true);
    console.log("after2");
});

console.log("end");
```

The above code uses `async/await`
