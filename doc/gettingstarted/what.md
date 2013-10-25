# What are the Reactive Extensions

Reactive Extensions (Rx) is a library for composing asynchronous and event-based programs using observable sequences and LINQ-style query operators.

RxJs is the JavaScript implementation of Rx. There are numerous other implementations targeting Python, C++, Java and the original implementation targeting .NET.

TODO Put TOC here



## Usages

RxJs is perfect for transforming, composing or querying asynchronous data streams as observable sequences. 
These sequences could have a single value, many values or even an infinite stream of data.

 * **Single Value Sequences** : web request, reading a files contents asynchronously, an asynchronous computation or another type of promise.
 * **Many value sequences** : a list of twitter followers, images from a web page, chat messages.
 * **Infinite sequences** : timers (`Observable.interval`), DOM events, tweets, instrumentation, geolocation events.

In this case, an infinite sequence is just where the source observable would never complete.

Usage of RxJs is equally relevant on the server applications as it is in GUI applications.
RxJs can be used in the GUI to provide responsive applications that animate, react to user events such as layout changes or key presses. 
See the [examples](../../examples) to see how to create [Autocomplete](../../examples#autocomplete "Autocomplete example") textboxes, [Image Croppers](../../examples#image-cropper), [Drag and Drop](../../examples#drag-and-drop) and more.
It is also easy to conceive of scenarios where non-GUI code can leverage RxJs, such as aggregating twitter feeds, querying live logs or [wrapping existing asynchronous APIs](../howdoi/wrap.md). 
RxJs even supplies a NodeJs bridge to allow you to use RxJs purely on the server-side.

## Guidelines and Patterns

### Formalization of 'Observable Sequence' definition
Rx is based on the Observer design pattern, however it further extends the pattern from just a simple callback model to an observable sequence paradigm.

The Observer pattern allows for zero-or-more observers(consumers) to subscribe to receive callbacks(values) from the observable subject(producer). 
The observers are able to unsubscribe from receiving callbacks and the observable subject may produce zero or more callbacks.   

The Observer pattern provides a useful base from which to start, however it fails to cater for concurrency concerns, finite event sources and failure scenarios. 

Rx extends the Observer pattern with the concept of an observable sequence.
An observable sequence can be formalized as : 

> A sequence of zero or more values, that optionally terminates. 
> A sequence can terminate either with or without an error.

With the concept of an *observable sequence*, a far richer platform is created. 
Instead on thinking of just callbacks, one can consider a sequence of values produced over time. 
These sequences can be transformed and queried.
Sequences can also be composed of other sequences.
Custom operators can be created to extend the provided (query, transformation & composition) operators.
By allowing both sequences and operators to be composed, Rx becomes a rich and powerful way to query event sources and asynchronous data flows.


### Transformation
An observable sequence may not always produce value in the form you wish to consume it. 
Rx provides ways to transform data from one form to another.

#### One to one 
Using the `select` operator, values can be mapped from one value to another. 
You may need to reduce a complex type like a `customer` to just the `customerId`. Alternatively you may enrich a simple type like `customerId` to a `customer` object.
Users of other functional languages might recognise the `select` operator as an equivalent to a *map* operator.

#### One to many
Some values from an observable sequence can be mapped to many values. 
For example using the `selectMany` operator, a sequence of twitter account values may be mapped to a sequence of tweets. If the sequence of accounts had three values and each account had 4 tweets, the result sequence would be a single sequence of 12 tweets.
This is analogous to a *flatMap* operator in other APIs.   

#### Many to one
Some sequences of values are more useful when they are aggregate many values into one value. 
For example; a count of tweets over a period of time, the sum of transactions, or perhaps the average price for a product. 

### Composition
Rx allows multiple sequences to be combined. 
Sequences can be combined in many fashions:

 - Sequentially : Concat, Repeat, StartWith
 - Concurrently : Amb, Merge, Switch
 - Pairing : CombineLatest, Zip, And-Then-When

Sequential combinators will process a sequence until it completes, and then subscribe to the next sequence as defined by the operator.

Concurrent combinators will subscribe to multiple sequences at the same time. 
The `Amb` operator will only return values from the sequence that is first to produce a value(callback). 
`Merge` will return all values from all sequences, flattened into a single output sequence. 
`Switch` allows you to consume an observable sequence of observable sequences, also referred to as nested sequences.

Pairing operators will ensure that values from two sequences are returned together as pairs. 
`Zip` will return the first values from each sequence as a pair, then the second values from each sequence as a pair and so on. 
`CombineLatest` will return the most recent values from each sequence as a pair. 
This allows for sequence to produce values at different rates. 

### Query

There are query operators that will allow you to filter and group results.
The `where` clause allows you to filter out values with a provided predicate.
The `skip` and `take` operators allow you to ignore initial or trailing values of a sequence. 
You can also group data (using `groupBy`) or get distinct values (using `distinct` and `distinctUntil`).


### Concurrency
While an an asynchronous API like RxJs can work without concurrency, it is a useful concern to control. 
As JavaScript implementations can adopt concurrent or multi-threaded environments, RxJs caters for this by providing Schedulers.
Schedulers enable you to queue a function to be performed on a known context.
This allows units of work to either be queued for execution asynchronously on the current thread. 
In a multi threaded environment, work can be queued to be run concurrently on another thread of execution.  
	
In some platforms, the introduction of concurrency can create a non-deterministic program.
RxJs avoids this by ensuring that its scheduler implementations are serialized.
This means that all of your work allocated to a scheudler will be processed in a serialised fashion.
This proves to be a safe way to construct programs and aids in the ability to rationalise and debug software.  

RxJs provides implementations to execute work based at a given time or at a given period.
This provides an excellent way to buffer values, determine timeouts or instantiate work at a given at a point in the future.


### Testing
With the introduction of concurrency operators, testability could be compromised. 
Rx addresses this by allowing the substitution of `Scheduler` instances with `TestSchedulers` that all you to emulate the concurrent nature of your code with *virtual time* while your tests remain single threaded and deterministic. 
 

### Cancellation
Cancellation is baked into to Rx.
Subscriptions to any observable sequence can be disposed (un-subscribed) which will avoid any future callbacks.
Additionally the cancellation will be issued to the observable sequence so that it may stop any IO or CPU bound work it was performing. 

### Custom operators

TODO


## Types
TODO

## History of Rx

Rx started as part of a research project at Microsoft under the code name Volta. 
In late 2009, early alpha releases had been published and the project picked up notoriety on Channel9.
Version 1 was released and continued to evolve with feedback from the community.
RxJs began its life as a port of Rx for JavaScript.
When version 2.0 was released in XXXX, the API was now stable. 
Due to it's success and now mature API, other ports of Rx started to appear. 


### What is Linq
[Linq](http://en.wikipedia.org/wiki/Language_Integrated_Query "Linq (Wikipedia)") has been one of the key building blocks that made the original implementation of Rx shine. 
By extending .NET to more easily embrace functional programming and fluent interfaces, the ability to query was built into the framework. 
Initially Linq was most popular as a method to query data at rest i.e. Collections, XML or Databases.
Rx adopted the same features, but instead offered the ability to query data in motion.

As JavaScript is a prototype language with functions as a first class type, adopting a Linq style API is relatively easy (compared to Java for instance). 
 
Linq is based on several key concepts:

#### Unitive
 
Linq unifies a querying language that can target numerous data sources: Objects/Collections, SQL, XML and event data sources.
Linq not only unifies querying across data sources, but also across languages.
Linq has been ported from .NET (C#, VB.NET, F# etc.) to C++, Python, Java and also JavaScript.

#### Extensible

You can extend Rx with your own custom query operators (extension methods).

#### Declarative

LINQ allows your code to read as a declaration of _what_ your code does and leaves the _how_ to the implementation of the operators.

#### Composable

By embracing a functional programming concepts like anon functions and fluent interfaces, Linq provides a way to compose operators together to create complex queries from simple operators.
            
#### Transformative

Queries can transform their data from one type to another. 
A query might translate a single value to another value, aggregated from a sequence of values to a single average value or expand a single data value into a sequence of values. 