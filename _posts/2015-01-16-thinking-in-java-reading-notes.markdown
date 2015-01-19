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

Ch5: Access Control
------------------------------	

**Access control (or implementation hiding) is about "not getting it right the first time"**	
Motivation of Access Control: Sometimes we need refactoring. We want **"separate the things that change from the things that stay the same."** This is very important to libraries.	

Java has access specifiers, the levels of access control from "most access" to "least access" are public, protected, package access(which has not keyword, generally described as default), and private.	

###Package: the library unit.	
Each source-code file is a compilation unit. The file name should be the same as public class name. For each source-code file, there are only one public class.	After compile the source code, each .java file has a corresponding .class file. A library is a group of these class files.	
> If you use a **package** statement, it must appear as the first non-comment in the file. 	

If you follow the reversed Internet domain name convention, your package name will be unique and you'll never have a name clash.	

Java interpreter finds .class in the follow step: It finds the environment variable CLASSPATH. Start from that root, the interpreter will take the package name and replace each dot with a slash to generate a path name off of the CLASSPATH root. Then concatenated to the various entries in the CLASSPATH.	

**protected** keyword: If you create a new package and inherit from a class in another package, the only members you have access to are the public members of the original package. If you inherit from the same package, you can manipulate all the members that have package access. Sometimes the creator of the base class would like to take a particular member and grant access to derived classes but not the world in general. That's what protected does.	

###Interface and implementation	

Access control puts boundaries within a data type for two important reasons. The first is to establish what the client programmers can and can't use. The second is separate the interface from the implementation.	

###Class access	

If you want a class to be available to a client programmer, you use the public keyword on the entire class definition.	 Actually if there is no public class in a source file, you can name it whatever you want.	
A class cannot be private or protected. So you have only two choices for class access: package access or public.	
If you don't want anyone else to have access to that class, you can make all the constructors private, thereby preventing anyone but you, inside a static member of the class, from creating an object of that class.	

Ch6: Reusing Classes	
----------------------------------	

There are two ways to use the classes without soiling the existing code in Java.	The one is directly create an object and use it inside the new class, which is called composition. This is simply reusing the functionality of the code, not its form. The other approach is creates a new class as type of an existing class. This technique is called inheritance.	

###Composition syntax	

Four places to initialize references:

1. At the point the objects are defined.	
2. In the constructor for that class	
3. Right before you actually need to use the object. This is called lazy initialization.	
4. Use instance initialization, which is initialization statement quoted by "{}". It will be called every time an instance is created.	

Trick: You can put a main() method in every classes for easy testing.	

###Inheritance syntax	

####Initializing the base class	

> When you create an object of the derived class, it contains within it a subobject of the base class. And it's essential that the base-class subobject be initialized correctly, and there's only one way to guarantee this: Perform the base-class initialization. Java automatically inserts calls to the base-class constructor in the derived-class constructor.	

> If your class doesn't have default arguments, or if you want to call a base-class constructor that has an argument, you must explicitly write the calls to the base-class constructor using the **super** keyword and the appropriate argument list.	

###Delegation	

Delegation is midway between inheritance and composition. Java is not directly support this relationship.	

> You place a member object in the class you're building(like composition), but at the same time you expose all the methods from the member object in your new class(like inheritance).	

You need provide wrapper methods that wrap up the delegation object's methods.	

###Combining composition and inheritance	

####Guaranteeing proper cleanup	
When some objects require clean up before being disposed, we can use **try...finally**. The **finally** keyword guaranteed the statement in the finally block will be executed no matter what happens in try.	

> You must also pay attention to the calling order for the base-class and member-object cleanup methods in case one subobject depends on another. In general, you should follow the same form that is imposed by a C++ compiler on its destructors: First perform all of the cleanup work specific to your class, in the reverse order of creation.	

####Name hiding	

> If a Java base class has a method name that's overloaded several times, redefining that method name in the derived class will not hide any of the base=class versions. Thus overloading works regardless of whether the method was defined at this level or in a base class.	Or you can add **@Override** tag.	

###Choosing composition vs. inheritance	

> Composition is generally used when you want the features of an existing class inside your new class, but not its interface.	
	When you inherit, you take an existing class and make a special version of it. In general, this means that you're taking a general-purpose class and specializing it for a particular need.	

###Upcasting	

Inheritance means "The new class is a type of the existing class." Upcasting is always safe because you're going from a more specific type to a more general type.	

####Composition vs. inheritance revisited	

> One of the clearest ways to determine whether you should use composition or inheritance is to ask whether you'll ever need to upcast from your new class to the base class.	

###The **final** keyword	

####**final** data	

There are two reasons that use a constant:

1. It can be a compile-time constant that won't ever change.	
2. It can be a value initialized at run time that you don't want changed.	

In Java, the compile-time constant must be primitives and are expressed with the final keyword. A value must be given at the time of definition of such a constant.	

A field that is both static and final has only one piece of storage that cannot be changed.	

When **final** is used with object references, **final** makes the reference a constant. Once the reference s initialized to an object, it can never be changed to point to another object. This restriction includes arrays.	

> Java allows the creation of blank finals, which are fields that are declared as final but are not given an initialization value. In all cases the blank final must be initialized before it is used, and the compiler ensures this. You can either initialize final fields at definition or in constructors.	

Java allows you to make arguments **final** by declaring them as **final** in the argument list. This means that inside the method you cannot change what the argument reference points to.	

####**final** method	
Used for prevent overriding. 	

#####**final** and **private**	
> Any **private** methods in a class are implicitly **final**. But you can override **private** method. 	

> "Overriding" can only occur if something is part of the base-class interface. That is, you must be able to upcast an object to its base type and call the same method. If a method is private, it isn't part of the base-class interface. You haven't overridden the method; you've just created a new method.	

####**final** classes	
A **final** class cannot be inherited. The fields of a final class can be final or not. All methods in a **final** class are implicitly **final**.	

###Initialization and class loading	

####Initialization with inheritance	

1. When class loading, all the static fields are initialized. First the base-class, second the derived class.
2. Then code start from main.	All the primitives in this object are set to their default values and the object references are set to null.
3. Initialize an inherited class instance, first call base-class constructor.	
4. All the instance fields in inherited class are initialized		
5. Execute the rest of derived constructor.