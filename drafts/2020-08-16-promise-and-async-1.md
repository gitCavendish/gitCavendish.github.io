---
title: "On exploring Promise 1: a sense of the picture"
categories: [Programming]
tags: [programming]
---

```js
// tree
let rootPromise = Promise.resolve("I am the top one.")
rootPromise.then((previousValue) => console.log(previousValue + " sub-node one"))
rootPromise.then((previousValue) => console.log(previousValue + " sub-node two"))
```

---

I remember vividly when I first stumbled on the term "asynchronous", the first thing jumped into my head was it must have something to do with my mobile phone" since I often *synchronize* my phone with my Mac. And our general notion about [sychronization](https://en.wikipedia.org/wiki/Synchronization) is that is a process that coordinates different parts of something in unison. So it's easy to think `async` as "to make things not to happen at the same time", but this is a bit different from what `async` means in web development. As I kept learning programming, I had more and more terms about sync/async collected in my head, such as "concurrency", "process", "main thread", "promise", "async function". Then I knew I can't jump on a time machine then travel back to the happy days when I just knew about how to use `setTimeout()` and `setInterval()`. And this feeling culminated when I tried to understand how `Promise` works with JavaScript. And I did spend a lot of time trying to understand how `Promise` works.

Most learning materials about `Promise` out there focus on how to use `Promise` and how good it is. For people who just learned some basic use of `setTimeout()` and `setInterval()`, as well as some basic use of [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest), they just got a bit familiar and comfortable with these things. Then lots of people come to you and say "you know what, we've gotten a better tool to deal with async things, it's called `Promise`", after a while some other people tell you "you should try `async function` and `fetch` API, they are promise-based, they are awesome!". As a diligent student, you paid a lot of time reading through materials and going through the code examples again and again. But, you are still not so sure about how to use `Promise` as well as all the promise-based techniques. Why? I think there are several reasons:

- 1. don't have a correct definition or description of "async" in head, or think it as a very complicated concept
- 2. don't know how the browsers coordinate sync and async tasks
- 3. people who write introductions about `Promise` assume that all readers have known `1` and `2` mentioned above,  also some key facts about `Promise` are not noticed by beginners, therefore some mental gaps persist in the understanding of `Promise`.

I have to confest that we need patience to gain a well enough undestanding of `Promise`, because it relates to many other concepts. And it's almost impossible to understand a concept without knowing other related ones. As [Daniel Dennett](https://en.wikipedia.org/wiki/Daniel_Dennett) would say:

> "You can't believe a dog has four legs without believing that legs are limbs and four is greater than three, etc."

I'll try to write a 2-part post to explore these points. The first part will focus on general idea of `async` and mental model of "event loop", the second part will share some key facts I realized very late during my journey of learning `Promise`. I don't write too much about "how to use Promise", because there're many excellent materials about this on the internet, I think they do better than me.

### A simple understanding of "async"

#### A feeling about async

Some of us make breakfast as a daily routine. Although everyone has his/her own preference, but if you don't change your breakfast too often as I do, you may preprare it in a relatively fixed procedure. As a lazy one, most of the time I have these as my breakfast: a cup of drip coffee, 2 boiled eggs, 1 sweet potato. And here're what I need to do(with time consumptions) every morining:

- heat water for brewing coffee (4 mins
- brew drip coffee (3 mins
- boil eggs (10 mins
- heat potato(pre-boiled and frozen) with microwave oven (2 mins

Intuitively we probably won't do these tasks sequentially. For example, when we are boiling eggs, we won't stare at the pot seeing the water boiling gradually then wait another few minutes. Thus it may take us `3 + 4 + 10 + 2 = 19` minutes to make the breakfast. We all know we can do something else while we have started some previous tasks, especially some time-consuming tasks. Or we can say some tasks can be handled in parallel. One way to do things in this style may be like this:

```
|-------------- boil eggs --------------|
 |- heat potato -|
  |--- heat water ---|
                      |- brew coffee -|
```

By doing this we can have our breakfast only after 10 minutes. And this is similar to the general idea of ["asynchronous"](https://developer.mozilla.org/en-US/docs/Glossary/Asynchronous) in programming.

> Asynchronous software design expands upon the concept by building code that allows a program to ask that a task be performed alongside the original task (or tasks), without stopping to wait for the task to complete.   -- MDN

I'd prefer the definition of "asynchrony" on [wikipedia](https://en.wikipedia.org/wiki/Asynchrony_(computer_programming)):

> Asynchrony, in computer programming, refers to the occurrence of events independent of the main program flow and ways to deal with such events.

There're some common things in these two definitions. First there's an "original task" or "main program flow". Second, there's some other independent but related events or tasks need to be handled alongside the main one. And "async" is the way to deal with this situation. We can first apply an oversimplified understanding of how sync and async are coordinated.
That is, first handle the main task(the sync part), then the other ones(the async part).

#### an oversimplified solution: async goes after sync

A good starting point to develop a realization about the existence of sync and async in JavaScript is the "zero-delay" example with `setTimeout`:

```js
setTimeout(() => {
  console.log("I am the first line.");
}, 0);

// this line is only added for increase the time consuption in between
for(let i = 0; i < 1000000000; i += 1 ) {};

console.log("I am the last line.");
```

If we don't have any notion about async with JavaScript we may expect the two messages to be printed out sequentially, just as the order they were written in code. However in this case, though the time delay of `setTimeout` is set to `0`, plus that we add a time-consuming operation inbetween, but the message in `setTimeout` callback always goes after the `"I am the last line."`. This is a typical example to prove the existence of sync and async part in JavaScript.

An oversimplified description of how this works is: the async part is executed after the sync part has finished executing. The async part here is the callback passed to `setTimeout`, everything else is the sync part. But who makes the callback asynchronous? It's the `setTimeout` API. `Promise` also, has similar feature. Let's change the code example to put `Promise`:

```js
Promise.resolve("").then(() => console.log("I am the first line."));      // 1

// this line is only added for increase the time consuption in between
for(let i = 0; i < 1000000000; i += 1 ) {};                               // 2

console.log("I am the last line."); // this always gets printed out first // 3
```

The only change made here is the way we wrap the callback. And we get the messages printed out in the same order as the zero-delay one. For now we don't have to worry about what does the `Promise.resolve("")` do, just try to realize there is a distinction between sync and async, and the execution of sync and async code is coordinated in a certain way by the browser. It can be oversimplified as "async goes after sync".

#### why the separation of sync and async makes sense

Let's recall the line `for(let i = 0; i < 1000000000; i += 1 ) {};`. This line may take several or more seconds to run in browser, that's why the two messages are both printed out with a short delay. Since sync code is executed sequentially which means lines of code are executed one after another, if there's some code that may take a very long time to finish running, all the code after that will wait for it. If we apply this scenario to the script(just some code we write in a file) behind a webpage(or say a tab of the browser), when some sync code is continuously executing, the page just gets stuck and you'll find that you can do nothing with the page, it's just blocked. As we add more `0`s to `i < 1000000000`, the blocking time would increase at a substantial rate. It's just like the example of making breakfast, if all things have to be done one after another, if boiling eggs needed 2 hours, a lot of time could be wasted.

A sensible way is to start off all tasks as soon as possible, then outsource tasks that are time-consuming to somewhere else, just like how we change the way we make breakfast. Now take the code of incrementing `i` from `0` to `1000000000`, we can move it from the sync part to async part to eliminate the blocking experience. We can use `setTimeout` or `Promise` to do this:

```js
Promise.resolve("").then(() => console.log("I am the first line."));      // 1

// we can also do Promise.resolve("").then(() => for(let i = 0; i < 1000000000; i += 1 ) {});
setTimeout(() => { for(let i = 0; i < 1000000000; i += 1 ) {}}, 0);       // 2

console.log("I am the last line."); // this always gets printed out first // 3
```

Now the messages' printing order doesn't change, but the short period time of blocking disappears. Actually it doesn't disappear, it's just moved to the end of the execution. We can prove this by adding 2 or 3 more `0`s to the number then see if the browser is blocked after printing out the tow messages.

Many tasks can be time-consuming like the counting number one, others like retrieving data from remote server, processing large amount of data. The separation of sync and async is just really a way to optimize the coordination of different tasks to provide user a smoother experience.

Actually, the separation of sync and async are only made by humans conceptually, in fact they both are just code, a time-consuming calculation can be set to sync, a `console.log()` task can be set to async, it all depends on you, the person who writes the code.

Async code goes after sync code, but how these two parts are coordinated in the browser, how this task is achieved? The answer is the [event loop model](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop).

### Mental model of Event Loop -- the mechanism to coordinate sync and async tasks

Imagine "async code goes after sync code" a chunk of code in a function block, such as `function solution() { async code goes after sync code }`. We put this code into a loop, then we get the "Event Loop" such as `while(true) { solution() }`. Of course things are not so simple but it's also not so complicated.

Frist let's check 2 definitions of event loop.

According to [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop):

> JavaScript has a concurrency model based on an event loop, which is responsible for executing the code, collecting and processing events, and executing queued sub-tasks. This model is quite different from models in other languages like C and Java.

According to [whatwg](https://html.spec.whatwg.org/dev/webappapis.html#event-loops):

> To coordinate events, user interaction, scripts, rendering, networking, and so forth, user agents must use event loops as described in this section. Each agent has an associated event loop, which is unique to that agent.

Wow these terms are intimidating. I prefer understanding event loop from a more general sense, a good explanation is the video [What the heck is the event loop anyway?](https://www.youtube.com/watch?v=8aGhZQkoFbQ) by Philip Roberts.

Remeber that we can turn a piece of sync code to async by using APIs such as `setTimeout`, often "a piece of code" refers to a callback, which is essentially a function, and inside that function, is the tasks we want run asynchronously. There are different APIs or ways to set tasks as async. Once we know which tasks should be asynchronous, there must be a way to set aside them for later execution.

Different materials about event loop may mention different terms like "stack", "heap", "main thread", "queue", "task queue", "micro-task queue", "macro-task queue", some topics are lower level knowledge in computer science, and it's tempting to dig deeper on these things. But we should realize the "Event Loop" is not some concrete code, it's a mental model, an abstract model, it's written [in the standard](https://html.spec.whatwg.org/multipage/webappapis.html#event-loops), but there doesn't have a single right way to implement it. Of course you can learn about every implementation detail of event loop in one browser like say chrome, but things may be so different when you look into another browser. So what's in common -- is the event loop model. Despite all the different implementation details, the event loop model resides in various browsers operates the same way at a high level. If we understand event loop correctly at a high level, we can confidently predict how the sync and async code will be operated within an app, therefore we can write code confidently with that correct model in our mind.

#### Components of event loop

One important thing we need to clarify is the relationship among JavaScript language, the browser, and the event loop. The browser is more than JavaScript language. JavaScript is just a core component of the browser, it's like an engine, actually JavaScript only does synchronous things. The browser actually provides a whole suite of components to maintain an environment for the event loop model to be implemented. Let's zoom in to look at the event loop model.

There are certain components in an event loop:

- the main thread/stack: as the word "main" indicates, that's where we run our main tasks, or we can understand it as a place to run sync code
- a task queue: it's a place queued with tasks that are waiting to be executed in the main thread.
- web apis: the tools provided by the browser to schedule tasks sent from the main thread to the task queue

To put these components in operation, the event loop acts as an observer. He keeps an eye on the main thread, if all the tasks there are finished running, it let the oldest(the one that got queued earliest) task in the task queue pop out into the main thread, and then execute it, then the second earliest one, so on and so forth.

Let's review the code example:

```js
setTimeout(() => { console.log("I am the first line.") }, 0);          // 1

for(let i = 0; i < 1000000000; i += 1 ) {};                            // 2

console.log("I am the last line.");                                    // 3
```

Except for the callback passed to `setTimeout` at line 1, all other code is synchronous, which means they be executed first, line by line from top to bottom.

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ghx9nj4n95j319009mwfu.jpg)

#### run, event loop

Imagine that all code, sync and async, is first put in the main thread. When code goes to line 1, `setTimeout` will set the callback aside to a scheduler or say timer, then the code in the main thread goes on executing, when code in the main thread has finished running, the scheduler(timer) starts counting for `0` second, then the callback will be put in the task queue. The work of event loop is to look at the main thread, if all sync code has finished running there, the first(oldest) task got queued in the task queue will be popped out then pushed in the main thread and be executed. And this process keeps running as if it's a "loop".

The event loop model explains why zero-delay callback doesn't have a real zero delay. Because based on how the event loop operates, the real time delay is the never shorter than the `execution time of the main thread`.

Now look at the oversimplified understanding "Sync code gets executed before async code", it's actually not so wrong. This is even truer if you stick this sentence on a wheel.

### Conclusion

Now we know sync or async is not used to describe the property of code. It doesn't make too much sense to say things like "this function is async". "Async" is more of ways to coordinate different tasks, it's more of a choice made by programmer.  Event loop is the model that we can use to model how sync and async code will be coordinated in browsers. With these in our mind, we can start our journey towards `Promise`. And above the concept of async, Promises are all about making asynchronous code more readable and behave like synchronous code.





































### Why promise?

With the mental model of event loop in our minds, it would be easier to understand promise. But promise brings something new, such as the 3 states of promise, the promise chain, and [a whole set of rules describing how a promise resolves](https://promisesaplus.com/#the-promise-resolution-procedure). And some new techniques such as `async function`, `fetch api` are "promise-based" or at least "promise-related". So if we can't understand promise well enough , we would probably leave a big mental gap there.

As I did with event loop, I won't try to write a thorough section about how to use promise, since there're a few excellent articles about this topic. I just focus on some seemingly not-so-important facts that may impede the understanding of promise.

### About terms in this article

Based on different context, the word "promise" has different meanings, most of the difference can be distinguished with different writing forms but there're a few subtle ones may not be easily distinguished. In this post "promise" should may in the forms of:

- plain lowercase "promise": the general concept of promise
- code quoted lowercase `promise`: an instance of a promise
- code quoted uppercase `Promise`: the `Promise` [constructor](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/Promise)

### Sense of promise:

I want to start with several different definitions of promise. For now we don't have to understand all the terms before we can continue. Here comes the definitions:

- [Promise/A+](https://promisesaplus.com/): A promise represents the eventual result of an asynchronous operation. The primary way of interacting with a promise is through its then method, which registers callbacks to receive either a promise's eventual value or the reason why the promise cannot be fulfilled.
- [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise): A Promise is a proxy for a value not necessarily known when the promise is created. It allows you to associate handlers with an asynchronous action's eventual success value or failure reason.
- [wikipedia](https://en.wikipedia.org/wiki/Futures_and_promises): In computer science, future, promise, delay, and deferred refer to constructs used for synchronizing program execution in some concurrent programming languages. They describe an object that acts as a proxy for a result that is initially unknown, usually because the computation of its value is not yet complete.

So promise must have something to do with "async", and it's a representation/proxy for a future result. Bringing this high level sense into the exploring of the use of promise is very helpful.

### use of promise

As I said, this part of work is excellently done by some pros, thank them a lot! Inevitably you may come across unacquainted terms. You may want to simply check their definitions on wiki but don't go too deep, focus on "how to use promise".

- https://web.dev/promises/
- https://pouchdb.com/2015/05/18/we-have-a-problem-with-promises.html

### states of promise

Promise is like a wrapper for asynchronous operations(tasks), and it holds the result of the task and based on how things are going, it stipulates the result can be in one of three states:

- pending: the initial state, means the task is still processing and we don't know how things are going so far
- resolved: means the task is successfully fulfilled, and it may give us something we want such as data or just a message that indicates the task has succeeded.
- rejected: means the task failed, and reasonably a reason(often an error object) should be given to tell what was wrong

Generally speaking, `pending` is when the async operation is still processing, `resolved(fulfilled)` and `rejected` are when the async operation is completed whether succeeded or failed, when a `promise`'s state is `resolved(fulfilled)` or `rejected`, we also say it's settled.

### `Promise()` constructor is used to 'promisifying' something

There's a concise description about the purpose of `Promise` constructor.

> [The Promise constructor is primarily used to wrap functions that do not already support promises.](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/Promise)

After reading a lot about how to use promise, we are only left with the notion that `Promise()` can create a `promise` and `then` is the way to chain subsequent operations. But being aware the original designing purpose is also important, especially when you ask question like "Since `Promise()` and `then()` both return a promise, so what's the difference?"

https://stackoverflow.com/questions/31324110/why-does-the-promise-constructor-require-a-function-that-calls-resolve-when-co

https://stackoverflow.com/questions/22519784/how-do-i-convert-an-existing-callback-api-to-promises

> The Promise constructor is only used for promisifying1 asynchronous functions.

> then basically offers the ability to attach onFulfilled/onRejected callbacks to an existing promise

`then` is the way to access a `promise` or say to communicate with a `promise`

### Code in `Promise` executes as soon as the promise is created

I think this is an important fact but most intro level materials of `Promise` don't mention.  But actually it's not so obvious for a person who just switches from callback style to promise style.

Take the example we just write:

```js
function fetchURL(url) { // returns promise
  return new Promise((resolve, reject) => {
    let xhr = new XMLHttpRequest();
    xhr.open('GET', url);
    xhr.addEventListener('load', () => {
      let suburl = xhr.response[0];
      resolve(suburl);
    })
  });
};

fetchURL().then((suburl1) => fetchURL(suburl1)).then((suburl2) => fetchURL(suburl2)).then...
```

`fetchURL(url)` returns a promise that will be resolved with a new url `suburl`, then this new url will be passed to next `then`, then be used to get another url, so on and so forth. Since `fetchURL` always return a `Promise`, the chain will pause if one of `fetchURL` returns a `pending` promise. But what if we now have a list of urls `[u1, u2, u3]` that don't depend on each other, means the request to one url won't affect another, they are just different urls. And now if we want to get things from the 3 urls one after another, in the order of `1,2,3` we may write something like this:

```js
function requestURL(url) {
  return new Promise((resolve, reject) => {
    let xhr = new XMLHttpRequest();
    xhr.open('GET', url);
    xhr.addEventListener('load', () => {
      let result = xhr.response;
      resolve(result);
    })
  });
};

[u1, u2, u3].forEach(url => {
  requestURL(url)
})
```

Although all the requests may succeed but the order is not guaranteed. Why? Because `forEach` is sync and what we actually did can be seen as:

```js
requestURL(u1);
requestURL(u2);
requestURL(u3);
```

`requestURL` returns a `Promise`, inside the `Promise` constructor we write some async code. Due to the fact that the promise chain will be paused by `pending` promises, it's easy to transfer this fact(feeling) to a `Promise` constructor, thinking that the code within the constructor will somehow be paused till the previously created promises in the same scope is fulfilled. But:

- waitings only happen when there's `pending` promise in a chain. In other words, no chain, no wait, you can't just make a `Promise`, then pause it there.
- a `pending` promise never pauses. When a promise is created, its original state is `pending`, but `pending` doesn't mean waiting. As long as there are code towards `resolve` or `reject`, the task is running.

So making a bunch of `Promise`s doesn't mean the latter ones will wait for the early ones. Unless you wrap the operations of creating promise inside a function shell, then arrange them in a promise chain. There is a big difference between "creating a promise" and "a function that creates a promise". Because when we pass "a function that creates a promise", that promise won't be created before the chain advances to the current `then`, as well as the operations wrapped in the `Promise` constructor.

What we actually want to do is like:

`requestURL(u1).then(requestURL(u2)).then(requestURL(u3));`

How to streamline the requests in a wanted sequence? Also with `forEach`, but this time a bit different.

```js
let chain = Promise.resovle('');
[u1, u2, u3].forEach(url => {
  chain = chain.then(() => requestURL(url));
})
```

The purpose of making a resolved promise is to provide a starting promise, so we can make the chain. Notice `chain.then(() => requestURL(url))` is different from `chain.then(requestURL(url))`, the latter one will create a `Promise` and then the request inside the constructor is made immediately.


### `resolve` happens immediately

The `resolve` method don't know how much time a request would take. We call `resolve` in `Promise` constructor, an that happens inside the `'load'` event listener. Here `resolve(suburl)` has no notion about `sync/async` it's called immediately when the request is `'load'`ed, and calling `resolve(suburl)` grants the state `fulfilled` to the promise with `suburl` as its value to prepare for possible future operations. And resolving of a promise is synchronous or say happens instantly.

This may seem obvious after you've noticed it. But realizing this fact can fill some mental gaps while trying to understand the using of `Promise`. Since `Promise` is heavily about "async", it's easy to forget to differentiate that there're also "sync" things there. It's easy to grumble questions like "how does the promise know when to resolve itself", the answer is it doesn't know when. Because the "resolving" moment depends on something else such as:
  - explicit writing code to resolve `Promsise`, like `Promise.resolve()` or call `resolve` in a `Promise` constructor
  - being driven by events, such as `'load'`
  - other ways include using `fetch` or `async function` but to some extent they just hide the event-driven logic of resolving `Promise` under their more concise syntax

To me "`resolve` is sync" is a very useful nonsense.

#### 0 always pass function to `then`

This may seem obivous after we learned about the definition and use of `Promise`. But as days roll on, we may make some mistakes inside that pair of `parentheses` followed by `then`.

One common mistake is forget to pass function to `then()`. Say if we have a function:

```js
function onFulfilled(pre) {
  // do something with `pre`
}
```

And we may write things like:

```js
promise.then(onFulfilled("salt"))
```

Inside `( )` the `onFulfilled("salt")` is a function invocation, it's not a function, so what we pass to `then()` is the return value of executing `onFulfilled("salt")`. If `onFulfilled("salt")` returns string "salty cake" it's just like we are doing `promise.then("salty")`, which actually is doing `promise.then(() => ((x) => x)("salty"))`. Because based on [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then):

> If it is not a function, it is internally replaced with an ["Identity"](https://en.wikipedia.org/wiki/Identity_function) function (it returns the received argument).

[Promise/A+ spec](https://github.com/promises-aplus/promises-spec) also mentions that `then` must return a promise and if `onFulfilled` *is not a function*, a `then` called on a resolved promise must return a new promise resolved with the value of the previous promise. It's better to be expressed by code:

```js
let promise1 = Promise.resolve("One"); // we get a promise resolved with "One"
promise1.then("two"); // this returns a new promise: `Promise {<fulfilled>: "One"}` resolved with "One" NOT "Two".
```

In fact here what you actually want to do is `promise.then(() => onFulfilled("salt"))`.If `onFulfilled` is a function that wants to accept the previous promise' resolved value as its argument, we can write `promise.then((pre) => onFulfilled(pre))` or `promise.then(onFulfilled)`.

You may think if we pass a


If we make a promise chain with serveral non-functions inserted like:

`promise.then(func).then(non-func).then(func).then(non-func)`

You can imagine that you strikethrough the `.then(non-func)` parts in you mind like:  "promise.then(func)~~.then(non-func)~~.then(func)~~.then(non-func)~~"

```js
let promise1 = Promise.resolve("One"); // we get a promise resolved with "One"
let promiseR = Promise.resolve("Between") // another resolve promise

promise1.then(promiseR) // returns `Promise {<fulfilled>: "One"}`
```

If you want to get a new promise resolved with `"Between"`, just change `promise1.then(promiseR)` to `promise1.then(() => promiseR)`. Because we should always pass functions to `then`.

But if we change `promise1.then(() => promiseR)` to `promise1.then(function() { promiseR })`, we get `Promise {<fulfilled>: undefined}`. Why, because besides always passing functions to `then`, we should remember to always `return` within the functions, because:

- `then` alwasy returns a promise
- that promise' value is the return value of the function passed to `then`
- in JavaScript a function does not explicitly return will return `undefined`, this is why we got a promise resolved with `undefined` in the above example.

#### Nurture intimacy with standard

If you've ever explored some articles about promise, you may have been introduced with the [Promise/A+](https://promisesaplus.com/) standard, as it states, it's:

> An open standard for sound, interoperable JavaScript promises—by implementers, for implementers.

And as we roll down the page, there are just several sections of structured rules. So promise is more of a high level model, it's not a package of code. The rules describe how to implement promise, but there doesn't exist a single right way to implement it. This is very similar to what we talk about the mental model of event loop. Having this notion in our minds, in advance, it's less possible that we entangle ourselves with too much details, so that we can be more focused on the logic behind promise.

// ---------------------------------------------------------------------------------------------------------------------------------

### It's all about combination

### A word about language

Many of us may have heard an advice that how to name variables is important in programming. Naming of variables is actaully a matter of using better semantics. We can see this from the changing of `HTML` tag names, they become more and more descriptive and specific to tell others what an element is about, without too much explanation. In the case of understanding `Promise`, I remember several nouns referring to `Promise`, such as completion, proxy, representation, placeholder, object, etc.

It's up to you which noun can help you understand better. For example, some may get a aha moment when he/she stumbles upon "eventual completion", some other may think "future representation" is better. I think when learning programming, a concept is hard is not because it's new to us, normally it's because the concept involves to many related concepts, and it would be even harder if there're some related concepts you don't know well about.

// ---------------------------------------------------------------------------------------------------------------------------------

### Evolve from callback style to promise style(syntactic improvement)

<!-- Say if we want to send a request to retrieve some data from a server, we might write something like:

```js
function retrieveData(url) {
  let xhr = new XMLHttpRequest();
  xhr.open('GET', url);
  xhr.addEventListener('load', function() {
      let suburl = xhr.response[0];
      let xhr1 = new XMLHttpRequest();
      xhr1.open('GET', suburl);
      xhr1.addEventListener('load', function() {
          let suburl2 = xhr1.response[0];
          let xhr2 = new XMLHttpRequest();
          xhr2.open('GET', suburl2);
          xhr2.addEventListener('load', function() {
            let suburl3 = xhr2.response[0];
            // ...........
          })
          xhr2.send();
      })
      xhr1.send()
  })
  xhr.send();
}
```

This would be rewritten as this by `Promise` style:

```js
function fetchURL(url) { // returns promise
  return new Promise((resolve, reject) => {
    let xhr = new XMLHttpRequest();
    xhr.open('GET', url);
    xhr.addEventListener('load', () => {
      let suburl = xhr.response[0];
      resolve(suburl);
    })
  });
};

fetchURL().then((suburl1) => fetchURL(suburl1)).then((suburl2) => fetchURL(suburl2)).then...

// or

fetchURL().then(fetchURL).then(fetchURL).then...
``` -->


// ---------------------------------------------------------------------------------------------------------------------------------

<!-- ### The rhythm of promise chain

#### `pending` promises are pauses in the chain

`pending` is just what we call the period of time before a promise is settled.
Ways to insert pending promises in the chain.

- use `Promise` constructor
- use `async function` + `await`


#### how to make a `pending` promies?

A Promise is always pending before it settles.

  - don't resolve or reject it, then it keeps the state of `pending`, such as
     - before the `load` event
     - a function never call resolve or reject


#### how to make a settled promise?
  - `Promise.resolve()`, `Promise.reject()`
  - in constructor `new Promise()`
  - in `then`
    - returning something makes a resolved promise
    - throwing error makes a rejected promise -->

// ---------------------------------------------------------------------------------------------------------------------------------

### Key points about Promise

### wait from outside, wait from inside

#### `resolve` is sync(happens in a moment) "use of sync may not be appropriate here or not accurate"
  - resolve don't know how much time a request would take

We call `resolve` in `Promise` constructor, an that happens inside the `'load'` event listener. Here `resolve(suburl)` has no notion about `sync/async` it's called immediately when the request is `'load'`ed, and calling `resolve(suburl)` grants the state `fulfilled` to the promise with `suburl` as its value to prepare for possible future operations. And resolving of a promise is synchronous or say happens instantly.

This may seem obvious after you've noticed it. But realizing this fact can fill some mental gaps while trying to understand the using of `Promise`. Since `Promise` is heavily about "async", it's easy to forget to differentiate that there're also "sync" things there. It's easy to grumble questions like "how does the promise know when to resolve itself", the answer is it doesn't know when. Because the "resolving" moment depends on something else such as:
  - explicit writing code to resolve `Promsise`, like `Promise.resolve()` or call `resolve` in a `Promise` constructor
  - being driven by events, such as `'load'`
  - other ways include using `fetch` or `async function` but to some extent they just hide the event-driven logic of resolving `Promise` under their more concise syntax

To me "`resolve` is sync" is a very useful nonsense.

#### 0 always pass function to `then`

This may seem obivous after we learned about the definition and use of `Promise`. But as days roll on, we may make some mistakes inside that pair of `parentheses` followed by `then`.

One common mistake is forget to pass function to `then()`. Say if we have a function:

```js
function onFulfilled(pre) {
  // do something with `pre`
}
```

And we may write things like:

```js
promise.then(onFulfilled("salt"))
```

Inside `( )` the `onFulfilled("salt")` is a function invocation, it's not a function, so what we pass to `then()` is the return value of executing `onFulfilled("salt")`. If `onFulfilled("salt")` returns string "salty cake" it's just like we are doing `promise.then("salty")`, which actually is doing `promise.then(() => ((x) => x)("salty"))`. Because based on [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then):

> If it is not a function, it is internally replaced with an ["Identity"](https://en.wikipedia.org/wiki/Identity_function) function (it returns the received argument).

[Promise/A+ spec](https://github.com/promises-aplus/promises-spec) also mentions that `then` must return a promise and if `onFulfilled` *is not a function*, a `then` called on a resolved promise must return a new promise resolved with the value of the previous promise. It's better to be expressed by code:

```js
let promise1 = Promise.resolve("One"); // we get a promise resolved with "One"
promise1.then("two"); // this returns a new promise: `Promise {<fulfilled>: "One"}` resolved with "One" NOT "Two".
```

In fact here what you actually want to do is `promise.then(() => onFulfilled("salt"))`.If `onFulfilled` is a function that wants to accept the previous promise' resolved value as its argument, we can write `promise.then((pre) => onFulfilled(pre))` or `promise.then(onFulfilled)`.

You may think if we pass a


If we make a promise chain with serveral non-functions inserted like:

`promise.then(func).then(non-func).then(func).then(non-func)`

You can imagine that you strikethrough the `.then(non-func)` parts in you mind like:  "promise.then(func)~~.then(non-func)~~.then(func)~~.then(non-func)~~"

```js
let promise1 = Promise.resolve("One"); // we get a promise resolved with "One"
let promiseR = Promise.resolve("Between") // another resolve promise

promise1.then(promiseR) // returns `Promise {<fulfilled>: "One"}`
```

If you want to get a new promise resolved with `"Between"`, just change `promise1.then(promiseR)` to `promise1.then(() => promiseR)`. Because we should always pass functions to `then`.

But if we change `promise1.then(() => promiseR)` to `promise1.then(function() { promiseR })`, we get `Promise {<fulfilled>: undefined}`. Why, because besides always passing functions to `then`, we should remember to always `return` within the functions, because:

- `then` alwasy returns a promise
- that promise' value is the return value of the function passed to `then`
- in JavaScript a function does not explicitly return will return `undefined`, this is why we got a promise resolved with `undefined` in the above example.


#### 1 code in `Promise` executes as soon as the promise is created

I think this is an important fact but most intro level materials of `Promise` don't mention.  But actually it's not so obvious for a person who just switches from callback style to promise style.

Take the example we just write:

```js
function fetchURL(url) { // returns promise
  return new Promise((resolve, reject) => {
    let xhr = new XMLHttpRequest();
    xhr.open('GET', url);
    xhr.addEventListener('load', () => {
      let suburl = xhr.response[0];
      resolve(suburl);
    })
  });
};

fetchURL().then((suburl1) => fetchURL(suburl1)).then((suburl2) => fetchURL(suburl2)).then...
```

`fetchURL(url)` returns a promise that will be resolved with a new url `suburl`, then this new url will be passed to next `then`, then be used to get another url, so on and so forth. Since `fetchURL` always return a `Promise`, the chain will pause if one of `fetchURL` returns a `pending` promise. But what if we now have a list of urls `[u1, u2, u3]` that don't depend on each other, means the request to one url won't affect another, they are just different urls. And now if we want to get things from the 3 urls one after another, in the order of `1,2,3` we may write something like this:

```js
function requestURL(url) {
  return new Promise((resolve, reject) => {
    let xhr = new XMLHttpRequest();
    xhr.open('GET', url);
    xhr.addEventListener('load', () => {
      let result = xhr.response;
      resolve(result);
    })
  });
};

[u1, u2, u3].forEach(url => {
  requestURL(url)
})
```

Although all the requests may succeed but the order is not guaranteed. Why? Because `forEach` is sync and what we actually did can be seen as:

```js
requestURL(u1);
requestURL(u2);
requestURL(u3);
```

`requestURL` returns a `Promise`, inside the `Promise` constructor we write some async code. Due to the fact that the promise chain will be paused by `pending` promises, it's easy to transfer this fact(feeling) to a `Promise` constructor, thinking that the code within the constructor will somehow be paused till the previously created promises in the same scope is fulfilled. But:

- waitings only happen when there's `pending` promise in a chain. In other words, no chain, no wait, you can't just make a `Promise`, then pause it there.
- a `pending` promise never pauses. When a promise is created, its original state is `pending`, but `pending` doesn't mean waiting. As long as there are code towards `resolve` or `reject`, the task is running.

So making a bunch of `Promise`s doesn't mean the latter ones will wait for the early ones. Unless you wrap the operations of creating promise inside a function shell, then arrange them in a promise chain. There is a big difference between "creating a promise" and "a function that creates a promise". Because when we pass "a function that creates a promise", that promise won't be created before the chain advances to the current `then`, as well as the operations wrapped in the `Promise` constructor.

What we actually want to do is like:

`requestURL(u1).then(requestURL(u2)).then(requestURL(u3));`

How to streamline the requests in a wanted sequence? Also with `forEach`, but this time a bit different.

```js
let chain = Promise.resovle('');
[u1, u2, u3].forEach(url => {
  chain = chain.then(() => requestURL(url));
})
```

The purpose of making a resolved promise is to provide a starting promise, so we can make the chain. Notice `chain.then(() => requestURL(url))` is different from `chain.then(requestURL(url))`, the latter one will create a `Promise` and then the request inside the constructor is made immediately.

#### Nurture intimacy with standard

If you've ever explored some articles about promise, you may have been introduced with the [Promise/A+](https://promisesaplus.com/) standard, as it states, it's:

> An open standard for sound, interoperable JavaScript promises—by implementers, for implementers.

And as we roll down the page, there are just several sections of structured rules. So promise is more of a high level model, it's not a package of code. The rules describe how to implement promise, but there doesn't exist a single right way to implement it. This is very similar to what we talk about the mental model of event loop. Having this notion in our minds, in advance, it's less possible that we entangle ourselves with too much details, so that we can be more focused on the logic behind promise.

// ---------------------------------------------------------------------------------------------------------------------------------

### It's all about combination

### A word about language

Many of us may have heard an advice that how to name variables is important in programming. Naming of variables is actaully a matter of using better semantics. We can see this from the changing of `HTML` tag names, they become more and more descriptive and specific to tell others what an element is about, without too much explanation. In the case of understanding `Promise`, I remember several nouns referring to `Promise`, such as completion, proxy, representation, placeholder, object, etc.

It's up to you which noun can help you understand better. For example, some may get a aha moment when he/she stumbles upon "eventual completion", some other may think "future representation" is better. I think when learning programming, a concept is hard is not because it's new to us, normally it's because the concept involves to many related concepts, and it would be even harder if there're some related concepts you don't know well about.
