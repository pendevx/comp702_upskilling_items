# Javascript

Javascript is a programming language which is used primarily in the browser, but has expanded to be able to run in other runtime environments, such as Node.js, and Bun. It provides a powerful way to interact with the browser, and modify the DOM. 

Since I already have a fundamental understanding of Javascript, the main upskilling will be in the advanced areas of Javascript.

## Javascript's single-threaded concurrency model

Javascript is a single-threaded language, following a single-threaded concurrency model. Thus, there is only one main thread to execute tasks, and there must be a way to implement a concurrency model to complete asynchronous tasks.

Most other languages have a thread pool, and may directly call to the thread pool to get a thread and spin it up to run a specific task. However, Javascript, does not provide such ability. so the solution provided was an **event loop**.

## The Event Loop

The event loop is the main thread. It constantly polls a message queue for tasks, and runs them when they come in.

The event loop consists of multiple different queues. In the early days of web development, there were only two queues: a **microtask queue** and a **macrotask queue**. However, due to the development of the web, and the advancement of computers to be able to perform more powerful computations, the queues have split off into multiple different queues, following the browser's own preferences. However, one thing has not changed: the **microtask queue** has remained, and the WHATWG specifications specify that this queue must have the highest importance.

## How the event loop works

The browser internally may run multiple processes, and multiple threads within those processes. However, each browser tab generally has its own main thread, which performs all tasks ranging from the parsing of the DOM and CSSOM, rasterization, executing JS, layering, etc.

The event loop constantly polls the message queues for tasks. Based on order of importance, the event loop will always take tasks appended to the microtask queue first, until that queue is cleared, then based on the browser's internal implementation, will take messages from other queues based on what it considers a priority.

When a task has finished running, the event loop will then poll for the next message, and run that, and so on.

## How does the event loop support the concurrency model used in JavaScript?

Tasks are functions. 

When you register an event, for example a button's click event, you are subscribing a function (or *task*) to be executed when that event happens. The browser may then have a dedicated thread to listen to any interactivity events on the webpage, then send your function wrapped in a Message object into the correct queue when that event is fired. This is the fundamental basis of the event loop. 

Events in JavaScript are not limited to interactivity events, there are APIs provided by many of the objects, such as XMLHttpRequest, which allow you to hook your own listeners onto. And thus, the concurrency model is completed. When an event is fired, a function (task) you specified will be sent into the queue, and the main thread will pick up the task and execute it when available.

## Callback Hell

The approach of hooking event listeners may result in a developer nightmare and code smell called *Callback Hell*. Here is an example:

```js
asyncOperation(function (result1) {
    asyncOperation2(function (result2) {
        asyncOperation3(function (result3) {
            // More nested callbacks ...
        })
    })
})
```

The code above waits for `asyncOperation` to finish running, then it will pass the result into `result1`. However, since `asyncOperation2`'s execution depends on the result of `asyncOperation`, it needs to be nested within the callback passed into `asyncOperation`. The same goes for `asyncOperation3` and so on, causing heavily nested code of callback functions.
