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
<hr />

Ch4: Initialization & Cleanup	
----------------------------------

Overloading with primitives: A primitive can be automatically promoted from a smaller type to a larger one. If argument is larger than expected, it requires a narrowing conversion with a cast.	

The **this** keyword: if you are inside a method and you'd like to get the reference to the current object. The **this** keyword can be used only inside a non-static method, produces the reference to the object that the method has been called for. Call a method from a method in the same class, don't need to use this. The **this** keyword s also useful for passing the current object to another method. 	
	When calling constructors from constructors, you can simply use this(some arguments). It doesn't means the current object, but makes an explicit call to the constructor that matches that argument list.	


### Cleanup: finalization and garbage collection		


#### Understanding about GC:

+ Your objects might not get garbage collected. 
+ Garbage collection is not destruction	
+ Garbage collection is only about memory.	

finalize() does not called graranteed. But we can use finalize() to verify if the termination condition is satisfied.	

#### How a GC works.

+ Simple but slow GC technique is reference counting. Problem is for circular reference, it cannot detect the garbage.
+ Faster scheme based on the idea that any non-dead object must ultimately be traceable back to a reference that lives either on the stack or in static storage.	
+ JVM uses an adaptive GC scheme. One of the variants that being used is stop-and-copy, which mean first program is stopped, and copy all the live objects to another heap area, and leave garbage, the new heap area will be more compact. Problem 1 is copy back and forth is inefficient. Problem 2 is for stable program that seldom generate garbage, such a copy is useless. For problem 2, there is other scheme is called mark-and-sweep, which is slow when a lot of garbages exist.
+ mark-and-sweep: it also start from the stack and static storage and tracing to find live objects. If a live objects found, then mark this object. After mark process, then sweep dead objects.	
+ stop-and-copy reveals that this type of GC is not done in the background. When the garbage collection occurs, the program stopped.	
+ JVM's adaptive is it monitors the efficiency of each scheme. A large object has its own block. Each block has a generation count to keep track whether it is alive. In normal case, only the blocks created since the last GC are compacted; all other get their generation count bumped. Periodically, a full sweep is made: large objects are still not copied, just get generation bumped, and the blocks containing small objects are copied and compacted. When JVM found all objects are long-lived, it switch to mark-and-sweep mode. If the heap starts to become fragmented, it switch to stop-and-copy. 	
+ JIT partially or fully converts some code into machine code and thus runs much faster. It has two draw backs: one is takes a little more time at first, and added size to the executable, which might cause paging.	
+ Another approach is laze evaluation, which means the code is not JIT compiled until necessary.	

#### Member initialization	
If operate an uninitialized variable, will get an error, but they do have default initial value.	

#### Constructor initialization	

+ Order of initialization: Within a class, the order of initialization is determined by the order that the variables are defined within the class. The variables are initialized before any methods can be called-even the constructor.	
+ **static** data initialization: If you want to place initialization at the point of definition, it looks the same as for non-statics.	The order of initialization is **static** first, if they haven't already been initialized by previous object creation. Then the non-static	
+ Explicit **static** initialization: Java allows you to group other **static** initializations inside a special "**static** clause", sometimes called static block. **It appears to be a method, but it's just the static keyword followed by a block of code**. This code is executed only once: the first time you make an object of that class or the first time you access a static member of that class.	
+ There is a similar form for initializing non-static variables for each object. Just use {} quote all the initialize statement. It will be executed before constructor is called.	

### Enumerated types	
When a enum type created, the compiler added useful features to the type, like toString() print the name of an enum instance, ordinal() method to indicated the declaration order of a particular enum constant, values() returns an array of values of the enum constants in the order that they were declared. In fact, enums are classes and have their own methods.	
<hr />

Ch5: Access Control
------------------------------	

**Access control (or implementation hiding) is about "not getting it right the first time"**	
Motivation of Access Control: Sometimes we need refactoring. We want **"separate the things that change from the things that stay the same."** This is very important to libraries.	

Java has access specifiers, the levels of access control from "most access" to "least access" are public, protected, package access(which has not keyword, generally described as default), and private.	

### Package: the library unit.	
Each source-code file is a compilation unit. The file name should be the same as public class name. For each source-code file, there are only one public class.	After compile the source code, each .java file has a corresponding .class file. A library is a group of these class files.	
> If you use a **package** statement, it must appear as the first non-comment in the file. 	

If you follow the reversed Internet domain name convention, your package name will be unique and you'll never have a name clash.	

Java interpreter finds .class in the follow step: It finds the environment variable CLASSPATH. Start from that root, the interpreter will take the package name and replace each dot with a slash to generate a path name off of the CLASSPATH root. Then concatenated to the various entries in the CLASSPATH.	

**protected** keyword: If you create a new package and inherit from a class in another package, the only members you have access to are the public members of the original package. If you inherit from the same package, you can manipulate all the members that have package access. Sometimes the creator of the base class would like to take a particular member and grant access to derived classes but not the world in general. That's what protected does.	

### Interface and implementation	

Access control puts boundaries within a data type for two important reasons. The first is to establish what the client programmers can and can't use. The second is separate the interface from the implementation.	

### Class access	

If you want a class to be available to a client programmer, you use the public keyword on the entire class definition.	 Actually if there is no public class in a source file, you can name it whatever you want.	
A class cannot be private or protected. So you have only two choices for class access: package access or public.	
If you don't want anyone else to have access to that class, you can make all the constructors private, thereby preventing anyone but you, inside a static member of the class, from creating an object of that class.	
<hr />

Ch6: Reusing Classes	
----------------------------------	

There are two ways to use the classes without soiling the existing code in Java.	The one is directly create an object and use it inside the new class, which is called composition. This is simply reusing the functionality of the code, not its form. The other approach is creates a new class as type of an existing class. This technique is called inheritance.	

### Composition syntax	

Four places to initialize references:

1. At the point the objects are defined.	
2. In the constructor for that class	
3. Right before you actually need to use the object. This is called lazy initialization.	
4. Use instance initialization, which is initialization statement quoted by "{}". It will be called every time an instance is created.	

Trick: You can put a main() method in every classes for easy testing.	

### Inheritance syntax	

#### Initializing the base class	

> When you create an object of the derived class, it contains within it a subobject of the base class. And it's essential that the base-class subobject be initialized correctly, and there's only one way to guarantee this: Perform the base-class initialization. Java automatically inserts calls to the base-class constructor in the derived-class constructor.	

> If your class doesn't have default arguments, or if you want to call a base-class constructor that has an argument, you must explicitly write the calls to the base-class constructor using the **super** keyword and the appropriate argument list.	

### Delegation	

Delegation is midway between inheritance and composition. Java is not directly support this relationship.	

> You place a member object in the class you're building(like composition), but at the same time you expose all the methods from the member object in your new class(like inheritance).	

You need provide wrapper methods that wrap up the delegation object's methods.	

### Combining composition and inheritance	

#### Guaranteeing proper cleanup	
When some objects require clean up before being disposed, we can use **try...finally**. The **finally** keyword guaranteed the statement in the finally block will be executed no matter what happens in try.	

> You must also pay attention to the calling order for the base-class and member-object cleanup methods in case one subobject depends on another. In general, you should follow the same form that is imposed by a C++ compiler on its destructors: First perform all of the cleanup work specific to your class, in the reverse order of creation.	

#### Name hiding	

> If a Java base class has a method name that's overloaded several times, redefining that method name in the derived class will not hide any of the base=class versions. Thus overloading works regardless of whether the method was defined at this level or in a base class.	Or you can add **@Override** tag.	

### Choosing composition vs. inheritance	

> Composition is generally used when you want the features of an existing class inside your new class, but not its interface.	
	When you inherit, you take an existing class and make a special version of it. In general, this means that you're taking a general-purpose class and specializing it for a particular need.	

### Upcasting	

Inheritance means "The new class is a type of the existing class." Upcasting is always safe because you're going from a more specific type to a more general type.	

#### Composition vs. inheritance revisited	

> One of the clearest ways to determine whether you should use composition or inheritance is to ask whether you'll ever need to upcast from your new class to the base class.	

### The **final** keyword	

#### **final** data	

There are two reasons that use a constant:

1. It can be a compile-time constant that won't ever change.	
2. It can be a value initialized at run time that you don't want changed.	

In Java, the compile-time constant must be primitives and are expressed with the final keyword. A value must be given at the time of definition of such a constant.	

A field that is both static and final has only one piece of storage that cannot be changed.	

When **final** is used with object references, **final** makes the reference a constant. Once the reference s initialized to an object, it can never be changed to point to another object. This restriction includes arrays.	

> Java allows the creation of blank finals, which are fields that are declared as final but are not given an initialization value. In all cases the blank final must be initialized before it is used, and the compiler ensures this. You can either initialize final fields at definition or in constructors.	

Java allows you to make arguments **final** by declaring them as **final** in the argument list. This means that inside the method you cannot change what the argument reference points to.	

#### **final** method	
Used for prevent overriding. 	

##### **final** and **private**	
> Any **private** methods in a class are implicitly **final**. But you can override **private** method. 	

> "Overriding" can only occur if something is part of the base-class interface. That is, you must be able to upcast an object to its base type and call the same method. If a method is private, it isn't part of the base-class interface. You haven't overridden the method; you've just created a new method.	

#### **final** classes	
A **final** class cannot be inherited. The fields of a final class can be final or not. All methods in a **final** class are implicitly **final**.	

### Initialization and class loading	

#### Initialization with inheritance	

1. When class loading, all the static fields are initialized. First the base-class, second the derived class.
2. Then code start from main.	All the primitives in this object are set to their default values and the object references are set to null.
3. Initialize an inherited class instance, first call base-class constructor.	
4. All the instance fields in inherited class are initialized		
5. Execute the rest of derived constructor.	
<hr />

Ch7: Polymorphism	
------------------------------------	

> It provides another dimension of separation of interface from implementation, to decouple what from how.	

Polymorphism also called dynamic binding or late binding or run-time binding.	

Taking an object reference and treating ti as a reference to its base type is called upcasting.	

### The twist	

#### Method-call binding	

> Connecting a method call to a method body is called binding.	

> When binding is performed before the program is run, it's called early binding. 	

All method binding in Java uses late binding unless the method is static or final(private methods are implicitly final).	

#### Extensibility	

In a well-designed OOP program, most or all of your methods will follow the model and communicate only with the base-class interface. Such a program is extensible. The methods that manipulate the base-class interface will not need to be changed at all to accommodate the new classes.	

#### Pitfall: "overriding" **private** methods	
Only non-private methods may be overridden, so if the derived class has a same name method with a base-class private method, it is not override, it just a new method. And If you assign the derived class object to a base-class reference and call this method, it will directly call the base-classes private method.	

#### Pitfall: fields and **static** methods	
Only ordinary method calls can be polymorphic. **static** method doesn't behave polymorphically. **static** methods are associated with the class, and not the individual objects.	

### Constructors and polymorphism	

**Constructors are not polymorphic, they're actually static methods, but the static declaration is implicit.	

Order of constructor calls:

1. The base-class constructor is called. This step is repeated recursively such that the root of the hierarchy is constructed first, followed by the next-derived class, until the most-derived class is reached.	
2. Member initializers are called in the order of declaration	
3. The body of the derived-class constructor is called.	

#### Inheritance and cleanup	

When you have a clean method in base-class, you should override it in derived class and do some specific clean. When call the clean method in derived class, you should call the base-class cleanup method in reverse order of creation.	

When you share object with other objects, you maybe need reference counting before call clean method so that prevent memory leak.	

#### Behavior of polymorphic methods inside constructors	

> If you call a dynamically-bound method inside a constructor, the overridden definition for that method is used. However, the effect of this call can be rather unexpected because the overridden method will be called before the object is fully constructed. This can conceal some difficult-to-find bugs.	

### Covariant return types	

An overridden method in a derived class can return a type derived from the type returned by the base-class method. Before Java SE5, it will force to return a base-class return type in derived-class overridden method.	

### Design with inheritance	

#### Substitution vs. extension

About pure-inheritance or extend new method to derived class.	

#### Downcasting and runtime type information	
Java hase cast checking mechanism, The act of checking types at run time is called runtype identification (RTTI).	

<hr />

Ch8: Interfaces	
----------------------	

### Abstract classes and methods	

abstract method: This is a method that is incomplete; it has only a declaration and no method body.	

A class containing abstract methods is called an abstract class.	

Compiler ensures it is impossible to create an object of an abstract class.	

Derived-class should implement all the abstract methods if you want to make objects of the new type. Else, it is an abstract class.	

It's possible to make a class abstract without including any abstract methods. It prevent any instances of that class.	

### Interface	
The **interface** keyword produces a completely abstract class, one that provides no implementation at all.	

> An interface says, "All classes that implement this particular interface will look like this."	

> So the interfaces used to establish a "protocol" between classes.	

> However, an interface is more than just an abstract class taken to the extreme, since it allows you to perform a variation of "multiple inheritance" by creating a class that can be upcast to more than one base type.	

An interface can also contain fields, but these are implicitly **static** and **final**.	

When you implement an interface, the methods from the interface must be defined as public.	

### Complete decoupling	

If a method is work with a class not an interface, it is limited to work with the class inheritance hierarchy. But interfaces relaxes this constraint considerably.	

> Creating a method that behaves differently depending on the argument object that you pass it is called the Strategy design pattern.	The method contains the fixed part of the algorithm to be performed, and the Strategy contains the part that varies.	

If you cannot modify the library, you can use Adapter design pattern. 	
> In Adapter, you write code to take the interface that you have and produce the interface that you need.	

Decoupling interface from implementation allows an interface to be applied to multiple different implementations, and thus your code is more reusable.	

### "Multiple inheritance" in Java	

> In a derived class, you aren't forced to have a base class that is either **abstract** or "concrete". If you do inherit from a non-interface, you can inherit from only one. All the rest of the base elements must be interfaces. You place all the interface names after the **implements** keyword and separate them with commas. You can have as many interfaces as you want. You can upcast to each interface, because each interface is an independent type.	

Reason for interfaces:

1. Can upcast an object to more than one base type.	
2. Can prevent the client programmer from making an object of this class and to establish that it is only an interface	

### Extending an interface with inheritance	

You get a new interface when extending an interface. By extending interface, you can add new methods to interface, or combining different interfaces into a new interface. Interface support multiple inheritance.	

#### Name collisions when combining interfaces: should avoid using identical method names.	

### Adapting to an interface	

> Because you can add an interface onto any existing class in  this way, it means that a method that takes an interface provides a way for any class to be adapted to work with that method. This is the power of using interfaces instead of classes.	

### Fields in interfaces	

All fields in an interface are automatically **static** and **final**, the interface is a convenient tool for creating groups of constant values.	

#### Initializing fields in interfaces	

Fields defined in interfaces cannot be "blank **final**s", but they can be initialized with non-constant expressions.	

The fields are not part of the interface. The values are stored in the static storage area for that interface.		

### Nesting interfaces	

Interfaces may be nested within classes and within other interfaces.	

A nested interface in a class can be private.	

If an inner class that implements a private interface, that inner class can only be used as itself. 	
> You are not allowed to mention the fact that it implements the private interface, so implementing a private interface is a way to force the definition of the methods in that interface without adding any type information (that is, without allowing any upcasting)	

The only way to use an private interface instance is to hand the object to another object that has permission to use it.	

All the interfaces that nested in an interface is forced to be public.	

> When you implement an interface, you are not required to implement any interfaces nested within. Also, **private** interfaces cannot be implemented outside of their defining classes.	

### Interfaces and factories		

An interface is intended to be a gateway to multiple implementations, and a typical way to produce objects that fit the interface is the Factory Method design pattern.	

A class implements some service through interface, and an additional class implements service factory interface corresponding to the service interface to produce objects that providing services. Then when you create method that accept objects that implement service factory interface, you can send different service factory objects to that method, and use the service factory object produce new objects that provide specific services. This design pattern enable your code completely isolated from the implementation of the interface, thus making it possible to transparently swap on implementation for another.	

### Summary	

> An appropriate guideline is to prefer classes to interfaces. Start with classes, and if it becomes clear that interfaces are necessary, then refactor. Interfaces are a great tool, but they can easily be overused.	

<hr />

Ch9: Inner Classes	
------------------------------------

It's possible to place a class definition within another class definition. This is called an inner class.	

### Creating inner classes	
Placing the class definition inside a surrounding class.	
> If you want to make an object of the inner class anywhere except from within a **non-static** method of the outer class, you must specify the type of that object as <i>OuterClassName.InnerClassName</i>.	

### The link to the outer class	

When you create an inner class, an object of that inner class has a <i>link to the enclosing objet that made it</i>, and so it can access the members of that enclosing object - without any special qualifications. In addition, inner classes have access rights to all the elements in the enclosing class.	

The inner class will capture a reference to a particular object of the enclosing class. Then, when refer to a member of the enclosing class, that reference is used to select that member. An inner class can be created only in association with an object of the enclosing class. Construction of the inner-class object requires the reference to the object of the enclosing class, and the compiler will take care this.	

### Using **.this** and **.new**	

> If you need to produce the reference to the outer-class object, you name the outer class followed by a dot and **this**. This reference is known and checked at compile time, so there is no runtime overhead. 	

> Sometimes you want to tell some other object to create an object of one of its inner classes. To do this you must provide a reference to the other outer-class object in the **new** expression, using the **.new** syntax.	

It's not possible to create an object of the inner class unless you already have an object of the outer class. However, if you make a <i>nested class</i>(a **static** inner class), then it doesn't need a reference to the outer-class object.	

#### Inner classes and upcasting	

We can implement some interfaces using private or protected inner classes. When using these interfaces, we can upcast the inner class objects to interfaces references, and these inner classes are completely hide from outside.	

> The **private** inner class provides a way for the class designer to completely prevent any type-coding dependencies and to completely hide details about implementation.	

#### Inner classes in methods and scopes		

An inner class is created within the scope of a method. This is called a <i>local inner class</i>.	 This class cannot be accessed outside of the method. The class defined inside a method doesn't mean that class cannot have object reference once the method returns.	

The class can only be used inside the scope of its definition. However, it is created with everything else, just cannot access until inside the scope.	

### Anonymous inner classes	

Defined along with some object initialization, and return the type of creation required type.	

> If you're defining an anonymous inner class and want to use an object that's defined outside the anonymous inner class, the compiler requires that the argument reference be **final**.	

You can still use instance initialization statement partly implement some constructor functions.	

> Anonymous inner classes are somewhat limited compared to regular inheritance, because they can either extend a class or implement an interface, but not both. And if you do implement an interface, you can only implement one.

### Nested classes	

If you don't need a connection between the inner-class object and the outer-class object, then you can make the inner class **static**. This is commonly called a <i>nested class</i>.	

> A nested class means:	
	1. You don't need an outer-class object in order to create an object of a nested class.			
	2. You can't access a non-static outer-class object from an object of a nested class.	

A nested class can have static date, static fields, or nested classes. An ordinary non-static inner class cannot have all of above.	

Outer class has full access to the inner class methods and variables, no matter they are public or private.	

#### Classes inside interfaces	

> Normally, you can't put any code inside an interface, but a nested class <i>can</i> be part of an interface. Any class you put inside an interface is automatically **public** and **static**.		

#### Reaching outward from a multiply nested class	

> It doesn't matter how deeply an inner class may be nested - it can transparently access all of the members of all the classes it is nested within.		

> The "**.new**" syntax produces the correct scope, so you do not have to qualify the class name in the constructor call.

### Why inner classes?

The most compelling reason for inner classes is:	

> Each inner class can independently inherit from an implementation. Thus, the inner class is not limited by whether the outer class is already inheriting from an implementation.	

So Java does support multiple inheritance, but in another way.

With inner classes you have these additional features:	

1. The inner class can have multiple instances, each with its own state information that is independent of the information in the outer-class object.	
2.In a single outer class you can have several inner classes, each of which implements the same interface of inherits from the same class in a different way. 
3. The point of creation of the inner-class object is not tied to the creation of the outer-class object.	
4. There is no potentially confusing "is-a" relationship with the inner class; it's a separate entity.	

#### Closures & callbacks	

A closure is a callable object that retains information from the scope in which it was created. From  this definition, you can see that an inner class is an object-oriented closure, because it does't just contain each piece of information from the outer-class object, but it automatically holds a reference back to the whole outer-class object, where it has permission to manipulate all the members, even private ones.

> One of the most compelling arguments made to include some kind of pointer mechanism in Java was to allow callbacks. With a callback, some other object is given a piece of information that allows it to call back into the originating object at some later point.	

#### Inner classes & control frameworks	

> An <i>application framework</i> is a class or a set of classes that's designed to solve a particular type of problem. To apply an application framework, you typically inherit from one or more classes and override some of the methods. The code that you write in the overridden methods customizes the general solution provided by that application framework in order to solve your specific problem. This is an example of the <i>Template Method</i> design pattern.	

> A control framework is a particular type of application framework dominated by the need to respond to events.	

**A system that primarily responds to events is called an <i>event-driven system</i>.**	

A common problem in application programming is the graphical user interface, which is almost entirely event-driven.	

### Inheriting from inner classes	

When inheriting from inner class, you must initialize an outer class object and send it into inherited class' constructor. You must use the syntax:	

	enclosingClassReference.super();	

inside the constructor.	

### Can inner classes be overridden?	

No. The only way is explicitly add "extends" to the inner class of the inherited class.	

### Local inner classes	

Defined in the method body. It can access all the fields of the outer class. 	
The two reasons that you choose inner local class over anonymous inner class:	

1. You need a named constructor or an overloaded constructor	
2. You need more than one object of that class	

### Inner-class identifiers	

inner class .class file naming convention: enclosingClassName$innerClassName.class	

If inner classes are anonymous, the compiler simply starts generating numbers as inner-class identifiers.	

<hr />

Ch10: Holding Your Objects	
--------------------------------------	

When you cannot decide how much objects and what type they are you need to hold, you should use container.	

Java basic container classes type are **List, Set, Queue, and Map**.	

### Generics and type-safe containers	

With generics(using type parameters), you're prevented, at compile time, from putting the wrong type of object into a container.	

### Basic concepts	

The Java container library divides the "holding your objects" idea into two distinct concepts:	

1. **Collection**: a sequence of individual elements with one or more rules applied to them. 	

	> A **List** must hold the elements in the way that they were inserted, a **Set** cannot have duplicate elements, and a **Queue** produces the elements in the order determined by a queuing discipline(usually the same order in which they are inserted).		

2. **Map**: a group of key-value object pairs, allowing you to look up a value using a key.	

	> An **ArrayList** allows you to look up an object using a number, so in a sense it associates numbers to objects. A map allows you to look up an object using another object. It's also called an associative array, because it associates objects with other objects, or a dictionary, because you look up a value object using a key object just like you look up a definition using a word.

### Adding groups of elements	

**Arrays.asList(): taking either array of comma-separated list of elements, returns List object.**	
**Collections.addAll(): takes a Collection object or an array or a comma-separated list and adds the elements to the Collection.**	

Arrays.<type>asList() can tell the compiler what the actual target type should be for the resulting **List** type produced by **Arrays.asList()**. This is called an explicit type argument specification.	

### Printing containers	

+ Print array: Arrays.toString()	
+ Print collection & Map: direct print. A Collection is printed surrounded by square brackets, with each element separated by a comma. A Map is surrounded by curly braces, with each key and value associated with an equal sign.		

HashSet, TreeSet and LinkedHashSet are types of Set.	

+ The HashSet is the fastest way to retrieve elements, and as a result the storage order can seem nonsensical.	
+ TreeSet keeps the objects in ascending comparison order.	
+ LinkedHashSet keeps the objects in the order in which they were added.	

> HashMap provides the fastest lookup technique, and also doesn't hold its elements in any apparent order. A TreeMap keeps the keys sorted by ascending comparison order, and a LinkedHashMap keeps the keys in insertion order while retaining the lookup speed of the HashMap.	

### List	

There are two types of List:

+ The basic ArrayList, which excels at randomly accessing elements, but is slower when inserting and removing elements in the middle of a List.	
+ The LinkedList, which provides optimal sequential access, with inexpensive insertions and deletions from the middle of the List. A LinkedList is relatively slow for random access, but it has a larger feature set than the ArrayList.	

### Iterator	

An iterator is an object whose job is to move through a sequence and select each object in that sequence without the client programmer knowing or caring about the underlying structure of that sequence. 	

Iterators sometimes called unify access to containers.	

#### ListIterator	

> The ListIterator is a more powerful subtype of Iterator that is produced only by List classes. While Iterator can only move forward, ListIterator is bidirectional. It can also produce the indexes of the next and previous elements relative to where the iterator is pointing in the list, and it can replace the last element that it visited using the set() method. You can produce ListIterator pointing at the beginning by calling listIterator() or pointing at index n by calling listIterator(n).	

### LinkedList	

> LinkedList also adds methods that allow it to be used as a stack, a Queue or a double-ended queue(deque).	

### Stack	

Implemented based on LinkedList, which is better for story telling.	

### Set	

A Set determines membership based on the "value" of an object.	

TreeSet keeps elements sorted into a red-black tree data structure.	

LinkedHashSet also uses hashing for lookup speed, but appears to maintain elements in insertion order using a linked list.	

### Map	

### Queue	

offer() is one of the Queue-specific methods; it inserts an element at the tail of the queue if it can, or returns false/ Both peek() and element() return the head of the queue without removing it, but peek() returns null if the queue is empty and element() throws NoSuchElementException. Both poll() and remove() remove and return the head of the queue, but poll() returns null if the queue is empty, while remove() throws NoSuchElementException.	

#### PriorityQueue	

> A queuing discipline is what decides, given a group of elements in the queue, which one goes next. 	

Apriority queue says that the element that goes next is the one with the greatest need	

When you offer() an object onto a PriorityQueue, that object is sorted into the queue. The default sorting uses the natural order of the objects in the queue, but you can modify the order by providing your own Comparator. 	

### collection vs. Iterator	

Collection is the root interface that describes what is common for all sequence containers. In addition, the java.util.AbstractCollection class provides a default implementation for a Collection, so that you can create a new subtype of AbstractCollection without unnecessary code duplication. 	

Commonality achieved from implementing interface or through iterators.	

> Producing an Iterator is the least-coupled way of connecting a sequence to a method that consumes that sequence, and puts far fewer constraints on the sequence class than does implementing Collection.	

### Foreach and iterators	

Foreach syntax works both array and Collection object.	

Foreach uses **Iterable** interface to move through a sequence.	

**Iterable** interface has **iterator()** method, which returns an instance of an anonymous inner implementation.	

All **Collection** classes except **Map** are being made **Iterable**.	

An array is not automatically **Iterable**.	

#### The <i>Adapter Method</i> idiom	

> When you have one interface and you need another one, and you cannot override the old one, writing an adapter solves the problem.	

**Arrays.asList()** produces a **List** object that uses the underlying array as its physical implementation. If you do anything to that **List** that modifies it, and you don't want the original array modified, you should make a copy into another container.	

<hr />

Ch11: Error Handling with Exceptions	
------------------------------------------------------------	

### Concepts	

Exception handling makes code clearer and much readable.	

### Basic exceptions	

An <i>exceptional condition</i> is a problem that prevents the continuation of the current method or scope.	

When you throw an exception, several things happen.	

1. The exception object is created in the same way that any Java object is created.	
2. Current path of execution is stopped and the reference for the exception object is ejected from the current context.	
3. Exception-handling mechanism takes over and begins to look for an appropriate place to continue executing the program. This appropriate place is the <i>exception handler</i>, whose job is to revocer from the problem so the program can either try another tack or just continue.	

#### Exception arguments	

Every standard exception class has two constructors, the default one and another one which takes a string as argument. 	

When **throw** an exception, you give the resulting reference to **throw**. The exception object is ""returned" from the method.	The place where exception returned to is different to the place where normal execution return. 	

### catching an exception	

<i>Guarded Region</I>	
This is a section of code that might produce exceptions and is followed by the code to handle those exceptions.	

#### The **try** block	
If you're throw an exception inside a method, that method will stop execution and return to exception handler; if you don't want to exit, you should use **try** block.	

#### Exception handlers	
Exception handlers immediately follow the **try** block and are denoted by the keyword **catch**.	

### Creating your own exceptions	

	e.printStackTrace();	

The default version will print the stack information to standard error stream.	

#### Exceptions and logging	

java.util.logging.logger	

Call the static Logger.getLogger("") method get a logger, then use a stringwriter wrapped around by a printwriter, send to e.printStackTrace to convert the stack trace into string. Then call logger's severe level method and write the stack trace into logging.	

### The exception specification	

<i>exception specification</I> is part of the method declaration, appearing after the argument list. This specification uses an additional keyword, **throws**, followed by a list of all the potential exception types. If no exceptions are specified, then it default throws **RuntimeException**, which can be thrown anywhere without exception specifications.	

> Exceptions that are checked and enforced at compile time are called <i>checked exceptions</I>.	

### Catching any exception	

#### The stack trace	

> The information provided by **printStackTrace()** can also be accessed directly using **getStackTrace()**. This method returns an array of stack trace elements, each representing one stack frame. Element zero is the top of the stack, and is the last method invocation in the sequence.	

#### Re-throwing an exception	

> Re-throwing an exception causes it to go to the exception handlers in the next higher context. Any further **catch** clauses for the same **try** block are still ignored. 	
	If you simply re-throw the current exception, the information that you print about that exception in **printStackTrace()** will pertain t the exception's origin, not the place where you re-throw it. If you want to install new stack trace information, you can do so by calling **fillInStackTrace()**, which returns a **Throwable** object that it creates by stuffing the current stack information into the old exception object.	

#### Exception chaining	

<i>exception chaining</I>: catch one exception and throw another, but still keep the information about the originating exception.	

You can take a <i>cause</i> object in **Throwable** subclasses in their constructor.	

> Its' interesting to note that the only **Throwable** subclasses that provide the <i>cause</i> argument in the constructor are the three fundamental exception classes **Error, Exception, RuntimeException**. If you want to chain any other exception types, you do it through the **initCause()** method rather than the constructor.	

### Standard Java exceptions	

There are two general types of **Throwable** objects.	

+ **Error** represents compile-time and system errors that you don't worry about catching.	
+ **Exception** is the basic type that can be thrown from any of the standard Java library class methods and from your methods and runtime accidents.	

#### Special case: **RuntimeException**	

**RuntimeException**s are <i>unchecked exceptions</i>.	

Only exceptions of type **RuntimeException** can be ignored in your coding, since the compiler carefully enforces the handling of all checked exception.	

### Performing cleanup with **finally**	

#### what's **finally** for?	

> The **finally** clause is necessary when you need to set something <i>other</i> than memory back to its original state.	

**finally** can work with break and continue statements, which eliminates the need for a **goto** statement in Java.	

#### Using **finally** during **return**	

For Multiple **return**, we can use **finally** guarantee cleanup. (but the **return** statement must in try block).	

#### Pitfall: the lost exception	

Embed a try-finally block inside a try-catch block may cause exception of inner block totally lost.	
Also, using **return** inside the **finally** block will silence any thrown exception.	

### Exception restrictions	

> When you override a method, you can throw only the exceptions that have been specified in the base-class version of the method.	

This restriction does not apply to constructors.. However, since a base-class constructor must always be called one way or another, the derived-class constructor must declare any base-class constructor exceptions in its exception specification.	

A derived-class constructor cannot catch exceptions thrown by its base-class constructor.	

A derived-class version of method may choose not to throw any exceptions, even if the base-class version does. It will not break the exception mechanism when treated as base-class version.	

If an object is cast to base-class type, it forces to catch all the base-class exceptions.	

### Constructors	

The safest way to use a class which might throw an exception during construction and which requires cleanup is to use nested **try** blocks.	

### Exception matching	

When an exception is thrown, the exception-handling system looks through the "nearest" handlers in the order they are written. When it fins a match, the exception is considered handled, and no further searching occurs.	

A derived-class object will match a handler for the base class.	

Compiler does not allow using base-class catch clause to mask the derived-class exceptions.	

### Alternative approaches	

> One of the important guidelines in exception handling is "Don't catch an exception unless you know what to do with it."	

#### Passing exceptions to the console	

#### Converting checked to unchecked exceptions	

"Wrap" a checked exception inside a **RuntimeException** by passing it to the **RuntimeException** constructor.	

### Exception guidelines	

Use exceptions to:	

1. Handle problems at the appropriate level.	
2. Fix the problem and call the method that caused the exception again.	
3. Patch things and continue without retrying the method.	
4. Calculate some alternative result instead of what the method was supposed to produce.	
5. Do whatever you can in the current context and rethrow the same exception to a higher context.	
6. Do whatever you can in the current context and throw a different exception to a higher context.	
7. Terminate the program.	
8. Simplify.	
9. Make your library and program safer.	

<hr />	

Ch12: Strings	
-----------------------------------	

### Immutable **String**s	

Object of the **String** class are immutable.	

### Overloading '+' vs. **StringBuilder**	

When JVM do '+' with strings, it actually creates a new StringBuilder object and do append, then it call toString() to generate a final string. However, compiler cannot optimize for loop: it will generate a new StringBuilder object every loop for string concatenation.	

### Unintended recursion	

When concatenate object with a string inside its toString() method, it will cause infinite recursion call to toString() method.	

### Formatting output	

**printf(), System.out.format()** are in a similar way like C's printf function.	

The **Formatter** class is create an object that support formatting output like printf. More object-oriented.	

#### Format specifiers	

	 %[argument_index$][flags][width][.precision]conversion	

It is used in **Formatter** object.	

Control the minimum size of a field is accomplished by specifying a <i>width</i>. It will padding spaces if necessary. Default is right justified, can be overridden by including a '-' in the flag section.	

<i>precision</i> is used to specify maximum. For String, it tells the maximum chars will be display; for floating point numbers, it specify maximum number of decimal places to display. If the floating number's decimal exceeded the maximum, it will round to maximum, or appending zeros if not reach the maximum. It cannot apply to integers, will get exception when used for integer.	

#### **Formatter** conversions	

<table border="1" style="width:50%">
	<th colspan='2'>Conversion Characters</th>
	<tr>
		<th>d</th>
		<td>Integral (as decimal)</td>
	</tr>

	<tr>
		<th>c</th>
		<td>Unicode character</td>
	</tr>

	<tr>
		<th>b</th>
		<td>Boolean value</td>
	</tr>

	<tr>
		<th>s</th>
		<td>String</td>
	</tr>

	<tr>
		<th>f</th>
		<td>Floating point(as decimal)</td>
	</tr>

	<tr>
		<th>e</th>
		<td>Floating point(in scientific notation)</td>
	</tr>

	<tr>
		<th>x</th>
		<td>Integral(as hex)</td>
	</tr>

	<tr>
		<th>h</th>
		<td>Hash code(as hex)</td>
	</tr>

	<tr>
		<td>%</td>
		<td>Literal "%"</td>
	</tr>
</table>
For 'b' conversions, which means output as boolean value, it is valid for any argument type. For any non-boolean argument, as long as the argument type is not **null** the result is always **true**.	

#### **String.format()**	

This is used to create **String**s. Like C's **sprintf()**.	
> **String.format()** is a **static** method which takes all the same arguments as **Formatter's format()** but returns a **String**. It can come in handy when you only need to call **format()** once.	

### Regular expressions	

#### Basics
In Java, if you want to use '\d' to represent one or more digits, you should use '\\d'. If you want to insert a literal backslash, you say '\\\\'.	

To indicate "one or more of the preceding expression", you use a '+'.	

String class has built in `.matches()` method.	

String's `split` method means "Split this string around matches of the given regular expression."	

`replaceFirst`, `replaceAll` can replace the first occurrence or all of them.	

#### Quantifiers	

A <i>quantifier</i> describes the way that a pattern absorbs input text	

+ <i>Greedy</i>: A greedy expression finds as many possible matches for the pattern as possible.	

+ <i>Reluctant</i>: Specified with a question mark, this quantifier matches the minimum number of characters necessary to satisfy the pattern. <i>don-greedy</i>	

+ <i>Possessive</i>: Currently only available in Java. Possessive quantifiers do not keep intermediate states, and prevent backtracking.	

#### **Pattern** and **Matcher**	

Using `java.util.regex` library, call `static Pattern.compiler()` method first to produce a **Pattern** object. Then call `matcher()` method, passing the string that you want to search. 

> The `matcher()` method produces a **Matcher** object, which has a set of operations to choose from.	

##### find()	

> `Matcher.find()` can be used to discover multiple pattern matches in the *CharSequence* to which it is applied.	

> `find()` is like an iterator, moving forward through the input string. `find(int index)` can be given an integer argument that tells it the character position for the beginning of the search - this version resets the search position to the value of the argument.	

##### Groups	

> Groups are regular expressions set off by parentheses that can be called up later with their group number. Group 0 indicates the whole expression match, group 1 is the first parenthesized group, etc.	

##### **start()** and **end()**	

**start()** returns the start index of the previous match, and **end()** returns the index of the last character matched, plus one.	

##### **Pattern** flags	

### Scanning input	

> A **StringReader** turns a **String** into a readable stream, and this object is used to create a **BufferedReader** has a **readLine()** method. 

Or you can use **Scanner** class.	

> By default, a **Scanner** splits input tokens along whitespace, but you can also specify your own delimiter pattern in the form of a regular expression.	

> There is one caveat when scanning with regular expressions. The pattern is matched against the next input token only, so if your pattern contains a delimiter it will never be matched.	

<hr />

Ch13: Type Information	
------------------------------------------	

Runtime type information (RTTI)	

### The need for RTTI	
Polymorphism requires RTTI.	

> With RTTI, you can ask a **Shape** reference the exact type that it's referring to, and thus select and isolate special cases.	

### The **Class** object	

Type information is represented by a special kind of object called the <i>Class object</i> at runtime.	

> In fact, the **Class** object is used to create all of the "regular" objects of your class.	

> There is one **Class** object for each class that is part of your program. That is, each time you write and compile a new class, a single **Class** object is also created (and stored, appropriately enough, in an identically named **.class** file).	

JVM use <i>class loader</i> to make an object of that class.	

> All classes are loaded into the JVM dynamically, upon the first use of a class. This happens when the program makes the first reference to a **static** member of that class. It turns out that the constructor is also a **static** method of a class.	

> All **Class** objects belong to the class **Class**. A **Class** object is like any other object, so you can get and manipulate a reference to it. One of the ways to get a reference to the **Class** object is the `static forName()` method, which takes a **String** containing the textual name of the particular class you want a reference for. It returns a **Class** reference.	

If it fail to load a class, it will throw a **ClassNotFoundException**.	

Two way to get a reference to the appropriate **Class** object: `Class.forName()` and if you already have an object of a class, you can call `getClass()` method of **Object** root class.	

The class that's being created with **newInstance()** must have a default constructor. 	

#### Class literals	

You can use <i>class literal</i> to produce the reference to the **Class** object. It looks like `ClassName.class`.	

> Class literals work with regular classes as well as interfaces, arrays, and primitive types. In addition, there's a standard field called **TYPE** that exists for each of the primitive wrapper classes.	

> It's interesting to note that creating a reference to a **Class** object using `.class` doesn't automatically initialize the **Class** object. There are actually three steps in preparing a class for use: 	
1. <i>Loading</i>, which is performed by the class loader. This finds the bytecodes and creates a **Class** object from those bytecodes.	
2. <i>Linking</i>. The link phase verifies the bytecodes in the class, allocates storage for **static** fields, and if necessary, resolves all references to other classes made by this class.	
3. <i>Initialization</i>. If there's a superclass, initialize that. Execute **static** initializers and **static** initialization blocks.	

> Initialization is delayed until the first reference to a **static** method or to a non-constant **static** field.	

Use `.class` syntax to get a reference to the class doesn't cause initialization. However, **Class.forName()** initializes the class immediately in order to produce the **Class** reference.	

> If a **static final** value is a "compile-time constant", that value can be read without causing the class to be initialized. However, making a field **static** and **final** does not guarantee this behavior.	

> If a **static** field is not **final**, accessing it always requires linking and initialization.	

#### Generic class references	

	Class intClass  = int.class;
	Class<Integer> genericIntClass = int.class;
	genericIntClass = Integer.class;
	intClass = double.class;
	// genericIntClass = double.class; // Illegal	

But `Class<Number> genericNumberClass = int.class;` is also illegal, because the **Integer Class** object is not a subclass of the **Number Class** object.	

To loosen the constraints when using generic **Class** references, you can use <i>wildcard</i>, which is the symbol '?'.	

> In order to create a **Class** reference that is constrained to a type or <i>any subtype</i>, you combine the wildcard with the **extends** keyword to create a <i>bound</i>. Like `Class<? extends Number> bounded = int.class;`	

When you use the generic syntax for **Class** objects: **newInstance()** will return the exact type of the object, rather than just a basic **Object**.	

#### New cast syntax	

Get a class object reference first, then call `.cast(someobject)` cast the object into this class type. 	
This syntax is used when you are writing generic code that cannot decide which type to cast until runtime.	

`Class.asSubclass()` allows you to cast the class object to a more specific type.	

### Checking before a cast	

A third form of RTTI, using **instanceof** keyword tells you if an object is an instance of a particular type.	

	someObject instanceof someClass/Type	

Use **instanceof** check type info before downcast.	

#### Using class literals	

Using class literals instead of using **instanceof**, can provide compile time check before use any Class object.	

#### A dynamic **instanceof**	

> The **Class.isInstance()** method provides a way to dynamically test the type of an object.	

#### Counting recursively	

`classObject.isAssignableFrom()` perform a runtime check to verify that the object that you've passed actually belongs to the hierarchy of interest.	

### Registered factories	

Using factory method and polymorphic, create objects based on need.	

### **instanceof** vs. **Class** equivalence	

**instanceof** equality means x is instance of some type or some derived type.
**Class** object equality means the two Class objects are the same type. Even though one of them is the base of another, they are not equal.	

### Reflection: runtime class information	

Some terms: 	

<i>Rapid Application Development</i>(RAD) in IDE: This is a visual approach to creating a program by moving icons that represent components onto a form.	

Reflection provides the mechanism to detect the available methods and produce the method names. 

> Another compelling motivation for discovering class information at run time is to provide the ability to create and execute objects on remote platforms, across a network. This is called <i>Remote Method Invocation</i>(RMI), and it allows a Java program to have objects distributed across many machines. 	

> When you're using reflection to interact with an object of an unknown type, the JVM will simply look at the object and see that it belongs to a particular class. Before anything can be done with it, the **Class** object must be loaded. Thus, the **.class** file for that particular type must still be available to the JVM, either on the local machine or across the network. So the true difference between RTTI and reflection is that with RTTI, the compiler opens and examines the **.class** file at compile time. Put another way, you can call all the methods of an object in the "normal" way. With reflection, the **.class** file is unavailable at compile time; it is opened and examined by the runtime environment.	

#### A class method extractor	

Reflection can show all the interfaces either inherited from base class or newly added.	

**getMethods()** and **getConstructors()** return an array of **Method** and array of **Constructor**, respectively. 	

> The constructor you see is the one that's automatically synthesized by the compiler. If you then make **ShowMethods** a **non-public** class, the synthesized default constructor no longer shows up in the output. The synthesized default constructor is automatically given the same access as the class.	

### Dynamic proxies	

> A proxy can be helpful anytime you'd like to separate extra operations into a different place than the "real object", and especially when you want to easily change from not using the extra operations to using them, and vice versa (the point of design patterns is to encapsulate change - so you need to be changing things in order to justify the pattern). 	

> You create a dynamic proxy by calling the **static** method **Proxy.newProxyInstance()**, which requires a class loader (you can generally just hand it a class loader from an object that has already been loaded), a list of interfaces (not classes or abstract classes) that you wish the proxy to implement, and an implementation of the interface **InvocationHandler**. The dynamic proxy will redirect all calls to the invocation handler, so the constructor for the invocation handler is usually given the reference to the "real" object so that it can forward requests once it performs its intermediary task.

### Null Objects	

We can make a Null Object which can response to all messages that the "real" object will respond to, but providing a way to present null-ness.	

The simplest way to do this is to create a tagging interface.	

	public interface Null {}	

In general, the Null Object will be a Singleton, so here it is created as a **static final** instance.	

### Interfaces and type information	

Using RTTI, we can discover a possible more specific type of an object, and cast an "interface" type object to a more narrower type object. 	

Reflection can help us call the methods that we cannot see from outside using `callHiddenMethod()`.	

<hr />	

Ch 14: Generics	
-------------------------	

"Generics" means "pertaining or appropriate to large groups of classes". 	

### Simple generics	

> One of the most compelling initial motivations for generics is to create <i>container classes</i>.	

> One of the primary motivations for generics is to specify what type of object a container holds, and to have that specification backed up by the compiler.	

#### A tuple library	

Tuple allows you to return multiple return values in a single function return statement. 	

> It is simply a group of objects wrapped together into a single object. The recipient of the object is allowed to read the elements but not put new ones in. (This concept is also called a <i>Data Transfer Object or Messenger</i>) 	

	public class TwoTuple<A, B> {
		public final A first;
		public final B second;
		public TwoTuple(A a, B, b) {
			first = a;
			second = b;
		}

		public String toString() {
			return  "(" + first + ", " + second + ")";
		}
	}

Declare first and second as **final** can prevent re-assign value.	


### Generic interfaces	

You cannot use primitives as type parameters.	

> The class itself may or may not be generic - this is independent of whether you have a generic method.	

> If a method is **static**, it has no access to the generic type parameters of the class.	

Simply place a generic parameter list before the return value to make a method generic.	

> Notice that with a generic class, you must specify the type parameters when you instantiate the class. But with a generic method, you don't usually have to specify the parameter types, because the compiler can figure that out for you. This is called <i>type argument inference</i>.  So calls to **f()** look like normal method calls, and it appears that **f()** has been infinitely overloaded.	

	// Initializing Generic Class
	ArrayLIst<Integer> list = new ArrayList<Integer>();

	// Define generic method	
	public <T> void f(T x) {
		System.out.println(x.getClass().getName());
	}	

	// Then you can call it like this	
	gm.f("");
	gm.f(1);
	gm.f(1.0);
	......	

#### Leveraging type argument inference	

> Type inference doesn't work for anything other than assignment. If you pass the result of a method call such as **New.map()** as an argument to another method, the compiler will not try to perform type inference. Instead it will treat the method call as though the return value is assigned to a variable of type **Object**.	

You can explicitly specify the type in a generic method. You place the type in angle brackets after the dot and immediately preceding the method name. 	

#### Varargs and generic methods	

	public static <T> List<T> makeList(T... args) {
		List<T> result = new ArrayList<T>();
		for (T item : args) {
			result.add(item);
		}

		return result;
	}

#### A generic method to use with **Generator**s.	

Use generator to fill a **Collection**.	

	public static <T> Collection<T>
	fill(Collection<T> coll, Generator<T> gen, int n) {
		for (int i = 0; i < n; ++i) {
			coll.add(gen.next());
		}

		return coll;
	}

#### A general-purpose **Generator**	

	public class BasicGenerator<T> implements Generator<T> {
		private Class<T> type;
		public BasicGenerator(Class<T> type) {
			this.type = type;
		}

		public T next() {
			try {
				// Assumes type is a public class
				return type.newInstance();
			} catch(Exception e) {
				throw new RuntimeException(e);
			}
		}

		// Produce a Default generator given a type token:
		public static <T> Generator<T> create(Class<T> type) {
			return new BasicGenerator<T>(type);
		}
	}


### Anonymous inner classes	

Generics can also be used with inner classes and anonymous inner classes. We can implement **Generator** interface using anonymous inner classes.	

### The mystery of erasure	

	Class c1 = new ArrayList<String>().getClass();
	Class c2 = new ArrayList<Integer>().getClass();
	c1 == c2 // True	

> There's no information about generic parameter types available inside generic code.	

Java generics are implemented using <i>erasure</i>.	

#### The C++ approach	

Calling a generic type object method in a generic class, C++ will check at compile time if this object has the method. Java cannot check this at compile time, so it checks <i>bound</i>. We can use `<T extends someClass>` to give a bound for generic type, otherwise the bound is Object type.

> We say that a generic type parameter <i>erases to its first bound</i> (it's possible to have multiple bounds). We also talk about the <i>erasure of the type parameter</i>. The compiler actually replaces the type parameter with its erasure.	

#### Migration compatibility	

It is not a language feature, but a compromise in the implementation.	

> In an erasure-based implementation, generic types are treated as secondclass types that cannot be used in some important contexts.	

#### The action at the boundaries	

> Note that using **Array.newInstance()** is the recommended approach for creating arrays in generics.	

> Even though erasure removes the information about the actual type inside a method or class, the compiler can still ensure internal consistency in the way that the type is used within the method or class.	

> All the action in generics happens at the boundaries - the extra compile-time check for incoming values, and the inserted cast for outgoing values. 	

### Compensating for erasure	

> Occasionally you can program around these issues, but sometimes you must compensate for erasure by introducing a <i>type tag</i>. This means you explicitly pass in the **Class** object for your type so that you can use it in type expressions.	

#### Creating instances of types	

In generic class, there are reasons that cannot create a new instance of type T: one is the type information has been erased; the other one is compiler compiler cannot verify that T has a default constructor.	

#### Arrays of generics	

We cannot directly create an array of generics using T[] array = new T[size], so we create an array of objects and cast it.	

	T[] array = (T[])new Objects[size];	

The array type that passing out from generic class is **Object[]**. 	

### Bounds	

	interface HasColor { Java.awt.Color getColor(); }	
	class Dimension { public int x, y, z;}	
	// class must be first, then interfaces, and you can have only one concrete class but multiple interfaces
	class ColoredDimension<T extends Dimension & HasColor> {}