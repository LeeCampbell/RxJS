# What are the Reactive Extensions

Reactive Extensions (Rx) is a library for composing asynchronous and event-based programs using observable sequences and LINQ-style query operators.

RxJs is the JavaScript implementation of Rx. There are numerous other implementations targeting Python, C++, Java and the original implementation targeting .NET.

<TODO Put TOC here />



## Usages

_TODO_


 

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

With the concept of an Observable Sequence, a far richer platform is created. 
Instead on thinking of just callbacks, one can consider a sequence of values produced over time. 
These sequences can be Transformed and Composed.


### Transformation
An Observable Sequence may not always produce value in the form you wish to consume it in. Rx provides ways to transform data from one form to another.

#### One to one 
Using the `select` operator, values can be mapped from one value to another. 
You may need to reduce a complex type like a `customer` to just the `customerId`. Alternatively you may enrich a simple type like `customerId` to a `customer` object.

#### One to many
Some values from an Observable Sequence can be mapped to many values. 
For example using the `selectMany` operator, a sequence of twitter account values may be mapped to a sequence of tweets.   

#### Many to one
Some sequences of values are more useful when they are aggregate many values into one value. 
For example a count of Tweets over a period of time, the sum of transactions, or perhaps the average price for a product. 

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
`Zip` will return the first values from each sequence as a pair, then the second sequence as a pair and so on. 
`CombineLatest` will return the most recent values from each sequence as a pair. 
This allows for sequence to produce values at different rates. 

### Query

### Concurrency
_TODO_
	
Serialization 

Timers 

### Testing
With the introduction of concurrency operators, testability could be compromised. 
Rx addresses this by allowing the substitution of `Scheduler` instances with `TestSchedulers` that all you to emulate the concurrent nature of your code with *virtual time* while your tests remain single threaded and deterministic.  



## Types


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
Linq has been ported from .NET (C#, VB.NET, F# etc) to C++, Python, Java and also JavaScript.

#### Extensible

You can extend Rx with your own custom query operators (extension methods).

#### Declarative

LINQ allows your code to read as a declaration of _what_ your code does and leaves the _how_ to the implementation of the operators.

#### Composable

By embracing a functional programming concepts like anon functions and fluent interfaces, Linq provides a way to compose operators together to create complex queries from simple operators.
            
#### Transformative

Queries can transform their data from one type to another. 
A query might translate a single value to another value, aggregated from a sequence of values to a single average value or expand a single data value into a sequence of values. 