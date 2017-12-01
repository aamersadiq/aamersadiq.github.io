---
title:  "Git Learning"
date:   2017-07-17 15:04:23
categories: [javascript]
tags: [git]
---
In this post, I will cover ...

### Async Type Comparsions
  
Callbacks
Promises
Observables
Number of results
0 to Infinite
0 or 1
0 to Infinite
States
Depends on arguments
resolved with optional value
rejected with error
next() with value
error() with error
completed()
Can be cancelled
No
Not yet*
Yes
When is work (e.g. network request) done?
As soon as callback is registered (“eager”)
As soon as promise is created (“eager”)
Only once subscribed to (“lazy”) for cold observables.
Composable?
Not at all
Somewhat
Very!

### What are Observables?
Collections over time
“an object that can send multiple values to a consumer” (from the spec)
Values are pushed to the consumer once it is subscribed
They are lazy (most of the time), only doing work when actually subscribed to (similar to IObservable in .NET)
Subscribers/consumers provide 3 callback functions:
next() - called 0 or more times prior to completion
error() - called once if fatal error, nothing else gets called afterwards on that observable
complete() - called once if Observable is finite (does complete). Is never called for infinite Observables.
Variable naming convention (“Hungarian notation”): 
const myObservable$: Observable<number> = Observable.of(1,2,3);

### What are the advantages of using them?
Very composable
Provides a single, consistent way to interact with any sort of asynchronous operation, whether one value or many are produced
Code using observables tends to express business logic and intent fairly clearly and succinctly
Due to their lazy nature, if you expose an observable that never gets subscribed to, nothing is ever actually done
E.g. no network requests are made

They can be cancelled by the consumer, even before they have done anything meaningful
E.g. a network request may be cancelled before it ever gets sent
You can retry them if they fail: makeNetworkRequest().retry(3)
Performance: because they push values to you, you know when there are changes without having to check for them
Angular 2 can take advantage of this to use faster change detection (see later slide)
They know how to clean up after themselves
When you unsubscribe, all resources are cleaned up
Best case: you only ever subscribe to observables using the async pipe
Functional: most observable code uses pure functions and thus works well with immutable data structures


### What about promises?
Promises have only 2 callbacks, for when they are successfully resolved, or an error occurred
A promise can push at most 1 value to the consumer
They are composable, but not nearly as much as observables in RxJS are
A promise is eager not lazy - work is initiated during promise creation whether anything consumes that promise or not, so they cannot easily be cancelled.
Much like .then() creates a new promise, RxJS operators return new Observables
An observable can be created from a promise, and vice versa
With the latter, only Observables that complete can be converted, and the promise is resolved with only the last emitted value.
Returning a promise locks you into only ever sending one value, whereas returning an observable gives you the option to return multiple values later if necessary
My advice: use observables wherever possible, for maximum composability


### Do I have to use Observables?


### Cold/Hot Observables
“Cold” observables replay all their values to consumers and/or start doing work when subscribed to
E.g. Http observable in Angular - every new subscriber creates a new network request
Many RxJS functions produce cold observables: Observable.of(), Observable.interval(), etc
“Hot” observables produce values whether subscribed to or not
Consumers only get values produced after they subscribe
Few observables are like this
Example: observables created from DOM events using Observable.fromEvent()
Not all observables are strictly one or the other. Some are considered “warm” because they do something on subscription (like a cold observable), such as do work or push one or more values, but then behave like a hot observable in that they push values that occur after subscription
E.g. The observables produced by the share() and publish____() operators

A cold observable is like a DVD of a movie (remember them?) - when you put in the disc, and play the movie (“subscribe” to it), you get exactly the same series of pictures and sounds sent to you as anyone else watching a DVD of that movie. If you stop it half way through (unsubscribe), the movie is effectively cancelled and doesn’t do anything further.

A hot observable is like watching a movie at the cinemas. If you turn up (subscribe) late, the movie starts without you and you miss some of the story. If you leave early (unsubscribe), the movie continues to play without you.

A slightly different explanation can be found here.

### Key RxJS Operators
.map((value) => newValue)
.filter((value) => boolean)
.distinctUntilChanged()
.take(1)
.scan() - like Array.reduce(), but evaluated for every value emitted
.flatMap((value) => Observable)
.switchMap((value) => Observable)
Observable.combineLatest(obs1$, obs2$, (value1, value2) => newValue)

And many, many more! See the docs at http://reactivex.io/
Interactive marble diagrams for some operators at http://rxmarbles.com/ (also an Android app)


