---
layout: post
title: "Reading Notes For \"Thinking in Java\""
categories: Notes
---

Ch1: Introduction to Objects
------------------------------------------
OOP is a programming abstraction. 	
A pure OOP abstraction:	

+ Everything is an object	
+ A program is a bunch of objects telling each other what to do by sending messages.
+ Each object has its own memory made up of their objects	
+ Every object has a type	
+ All objects of a particular type can receive the same messages.	

> An object has state, behavior and identity. - by Booch	

Object has interfaces. 
> The interface determines the requests that you can make for a particular object.	

An Object provides services. 
> Thinking of an object as a service provider helps to improve the cohesiveness of the object. High cohesion is a fundamental quality of software design.	

The hidden implementation draw a boundary between the class creators and the client programmers. 

Reusing the implementation. 

+ Placing an object inside a new class - composition. If the composition happens dynamically, it is called aggregation. Composition is referred as "has-a" relationship.	

Inheritance	

+ package data and functionality together by concept.
+ inheritance describes the similarity between types by using the concept of base types and derived types.	
+ "Since we know the type of a class by the messages we can send to it, this means that the derived class is the same type as the base class"	
+ Two way to differentiate new derived class from base class:	
	+ Add new methods
	+ Overriding the existing method	

Interchangeable objects with polymorphism	

+ Late binding. "When you send a message to an object, the code being called isn't determined until run time". Late binding in Java is default behavior, you don't have to use keywords.	
+ Treating a derived type as though it were its base type, is upcasting.	

The singly rooted hierarchy	

+ In Java, ultimate base class is Object.	
+ Make it easier to implement a garbage collector.	

Containers	

+ You don't know how many objects you're going to need to solve a particular problem, or how long they will last. And you also don't know how to store those objects. You need container or collection. 
+ It will expand its self whenever necessary.	

Parameterized types (generics)	
Treat them as object first, and then downcast to a specific type. Downcast error is runtime error. Use parameterized type tell container or else about the type info.	

Object creation & lifetime	
+ Java uses dynamic memory allocation, exclusively, except primitive types. All object are allocated at pool of memory call heap. Using new keyword build an object.	
+ A garbage collector is designed to take care of the problem of releasing the memory.	

Exception handling: dealing with errors	

+ An exception is an object that is "thrown" from the site of the error and can be "caught" by an appropriate exception handler designed to handle that particular type of error.	
+ Exception handling isn't an object-oriented feature.	

Concurrent programming	

+ Partition the problem into separately running pieces. separately running pieces are threads, and the general concept is called concurrency.	

Java and Internet	

+ Client-side programming: web form submission passes through the Common Gateway Interface(CGI)	

<hr />	

Ch2: Everything is an object
---------------------------------

Manipulate objects with references.	

Must create all the objects.	

+ Where the storage lives: registers, stack(directly controlled by processor via stack pointer. object references stored on the stack, but object not on stack), the heap(general-purpose poll of memory), constant storage, non-RAM storage(streamed objects, persistent objects)	
+ Special case: primitive types. Primitive type on the stack.BigInteger and BigDecimal are wrapper classes, no primitive analogue.	

Never need to destroy an object	

+ Scoping: 
	> Scope is determined by the placement of "{}". 	
	A variable defined within a scope is available only to the end of that scope. Cannot hide a variable in a larger scope.	
	Reference vanishes at the end of the scope. But object is collected by GC.	

Creating new data types: class: char type default initial value = '\u0000'(null)	

Build a Java program	

+ Name visibility: name space	
+ Using other components: import keyword	
+ "static" keyword: only want single piece of storage for a particular field; don't want method associate with any particular object of this class.	

Comments and embedded documentation	
> All of the Javadoc commands occur only within /** comments. The comments end with */ as usual. There are two ways to use Javadoc: Embed HTML or use "doc tags"	

Ch3: Operators
------------------

The comma operator: a number of statements separated by commas, and those statements will b evaluated sequentially.	
Foreach syntax: for use with arrays and containers. foreach will also work with any object that is **Iterable**	

Ch4: Initialization & Cleanup
----------------------------------

Overloading with primitives: A primitive can be automatically promoted from a smaller type to a larger one. If argument is larger than expected, it requires a narrowing conversion with a cast.	

The **this** keyword: if you are inside a method and you'd like to get the reference to the current object. The **this** keyword can be used only inside a non-static method, produces the reference to the object that the method has been called for. Call a method from a method in the same class, don't need to use this. The **this** keyword s also useful for passing the current object to another method. 	
	When calling constructors from constructors, you can simply use this(some arguments). It doesn't means the current object, but makes an explicit call to the constructor that matches that argument list.	

Cleanup: finalization and garbage collection	

Understanding about GC:

+ Your objects might not get garbage collected. 
+ Garbage collection is not destruction	
+ Garbage collection is only about memory.	

finalize() does not called graranteed. But we can use finalize() to verify if the termination condition is satisfied.	

How a GC works.

+ Simple but slow GC technique is reference counting. Problem is for circular reference, it cannot detect the garbage.
+ Faster scheme based on the idea that any non-dead object must ultimately be traceable back to a reference that lives either on the stack or in static storage.	
+ JVM uses an adaptive GC scheme. One of the variants that being used is stop-and-copy, which mean first program is stopped, and copy all the live objects to another heap area, and leave garbage, the new heap area will be more compact. Problem 1 is copy back and forth is inefficient. Problem 2 is for stable program that seldom generate garbage, such a copy is useless. For problem 2, there is other scheme is called mark-and-sweep, which is slow when a lot of garbages exist.
+ mark-and-sweep: it also start from the stack and static storage and tracing to find live objects. If a live objects found, then mark this object. After mark process, then sweep dead objects.	
+ stop-and-copy reveals that this type of GC is not done in the background. When the garbage collection occurs, the program stopped.	
+ JVM's adaptive is it monitors the efficiency of each scheme. A large object has its own block. Each block has a generation count to keep track whether it is alive. In normal case, only the blocks created since the last GC are compacted; all other get their generation count bumped. Periodically, a full sweep is made: large objects are still not copied, just get generation bumped, and the blocks containing small objects are copied and compacted. When JVM found all objects are long-lived, it switch to mark-and-sweep mode. If the heap starts to become fragmented, it switch to stop-and-copy. 	
+ JIT partially or fully converts some code into machine code and thus runs much faster. It has two draw backs: one is takes a little more time at first, and added size to the executable, which might cause paging.	
+ Another approach is laze evaluation, which means the code is not JIT compiled until necessary.	

Member initialization	
If operate an uninitialized variable, will get an error, but they do have default initial value.	

Constructor initialization	

+ Order of initialization: Within a class, the order of initialization is determined by the order that the variables are defined within the class. The variables are initialized before any methods can be called-even the constructor.	
+ **static** data initialization: If you want to place initialization at the point of definition, it looks the same as for non-statics.	The order of initialization is **static** first, if they haven't already been initialized by previous object creation. Then the non-static	
+ Explicit **static** initialization: Java allows you to group other **static** initializations inside a special "**static** clause", sometimes called static block. **It appears to be a method, but it's just the static keyword followed by a block of code**. This code is executed only once: the first time you make an object of that class or the first time you access a static member of that class.	
+ There is a similar form for initializing non-static variables for each object. Just use {} quote all the initialize statement. It will be executed before constructor is called.	

Enumerated types	
When a enum type created, the compiler added useful features to the type, like toString() print the name of an enum instance, ordinal() method to indicated the declaration order of a particular enum constant, values() returns an array of values of the enum constants in the order that they were declared. In fact, enums are classes and have their own methods.