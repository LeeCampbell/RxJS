# What are the Reactive Extensions

Reactive Extensions (Rx) is a library for composing asynchronous and event-based programs using observable sequences and LINQ-style query operators.

RxJs is the JavaScript implementation of Rx. There are numerous other implementations targeting Python, C++, Java and the original implementation targeting .NET.

<TODO Put TOC here />



## Usages

_TODO_


 

## Guidelines and Patterns

### Formalization of 'Observable Sequence' definition
Rx is based on the Observer design pattern, however it further extends the pattern from just a simple callback model to an observable sequence paradigm.

The Observer pattern allows for zero-or-more observers(consumers) to subscribe to receive callbacks(values) from the observable subject(producer). The observers are able unsubscribe from receiving callbacks and the observable subject may produce zero or more callbacks.   

The Observer pattern provides a useful base from which to start, however it fails to cater for concurrency concerns, finite event sources and failure scenarios. 

Rx extends the Observer pattern with the concept of an observable sequence.
An observable sequence can be formalized as : 

> A sequence of zero or more values, that optionally terminates. A sequence can terminate either with or without an error.

With the concept of an Observable Sequence, a far richer platform is created. Instead on thinking of just callbacks, one can consider a sequence of values produced over time. These sequences can be Transformed and Composed.


### Transformation
An Observable Sequence may not always produce value in the form you wish to consume it in. Rx provides ways to transform data from one form to another.

#### One to one 
Using the `select` operator, values can be mapped from one value to another. You may need to reduce a complex type like a `customer` to just the `customerId`. Alternatively you may enrich a simple type like `customerId` to a `customer` object.

#### One to many
Some values from an Observable Sequence can be mapped to many values. For example using the `selectMany` operator, a sequence of twitter account values may be mapped to a sequence of tweets.   

#### Many to one
Some sequences of values are more useful when they are aggregate many values into one value. For example a count of Tweets over a period of time, the sum of transactions, or perhaps the average price for a product. 

### Composition
Rx allows multiple sequences to be combined. Sequences can be combined in many fashions:

 - Sequentially : Concat, Repeat, StartWith
 - Concurrently : Amb, Merge, Switch
 - Pairing : CombineLatest, Zip, And-Then-When

Sequential combinators will process a sequence until it completes, and then subscribe to the next sequence as defined by the operator.

Concurrent combinators will subscribe to multiple sequences at the same time. The `Amb` operator will only return values from the sequence that is first to produce a value(callback). `Merge` will return all values from all sequences, flattened into a single output sequence. `Switch` allows you to consume an observable sequence of observable sequences, also referred to as nested sequences.

Pairing operators will ensure that values from two sequences are returned together as pairs. `Zip` will return the first values from each sequence as a pair, then the second sequence as a pair and so on. `CombineLatest` will return the most recent values from each sequence as a pair. This allows for sequence to produce values at different rates. 

### Concurrency
	Serialization, Timers, 

### Testing
With the introduction of concurrency operators, testability could be compromised. Rx addresses this by allowing the substitution of `Scheduler` instances with `TestSchedulers` that all you to emulate the concurrent nature of your code with *virtual time* while your tests remain single threaded and deterministic.  

### What is Linq


## Types


## History of Rx
 