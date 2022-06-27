# Design Patterns

## What it's all about ?
It's simply a general solution of a common problem is software design.

## History ?
After the GoF book where 23 patterns where defined, a lot of the patterns where discovered every time a recurrent problem happens and someone found a good solution for it.

## Why should I even bother ?
- They are tested and proved to work design, knowing them would ease software design for you
- They define a common language between software developers, you could just say: "Use the Strategy for that" and everyone will understand.

## Criticism of patterns
- They are created due to the lack of the necessary programming language abstraction. For example, the Strategy pattern can be implemented with a simple anonymous (lambda) function in most modern programming languages.
- They try to unify a solution for a set of problems which pushes people to not adapt them and use them as the are.
- Unjustified use: novice that just known design patterns tend to use everywhere because they think it's always good to use them. 

## Factory Method
### ...
### ...
### Applicability
Use the Factory Method when:
-  You don’t know beforehand the exact types and dependencies of the objects your code should work with.
-  You want to provide users of your library or framework with a way to extend its internal components.
-  You want to save system resources by reusing existing objects instead of rebuilding them each time.

## Oreilly class
### Singleton
Singleton makes sure there is only one instance of some object, it can be used for example to implement database pools, configuration objects where we want them to be shared across the application.

- It's not a global variable because it's an object and an object is defined by what it does not what it contains
- You can't observe it just by looking into code you have to know how it's been used to know if it's a singleton, a utility object (like java.math) or just a bunch of global variables
- Solving singleton problems with double checking sucks 
- It's not about the code, it's not about the structure, it's about how code behaves.

### Abstract Factory
An Abstrac class (a.k.a the abstract factory) has a method that returns another abstract class (a.k.a the abstract product) and the concrete factory tries as hard as possible to not reveal the implementation details about it's concrete product, In java it's done with a `private final class` inside the concrete factory class.
**Note:** Java has many many mecanisme that restricts/controls access to fields/methods ... (private, static, inner classes, ...) while Python does the exact opposite of that, Python literally exposes it's own implementations and allow you to do whatever you want and says: "We are all adults here!" [ref1](https://mail.python.org/pipermail/tutor/2003-October/025932.html), [ref2](https://python-guide-chinese.readthedocs.io/zh_CN/latest/writing/style.html#we-are-all-consenting-adults)

#### Adapters
It's used to swap libraries that does the same thing without changing the base code

#### Decorators
Add functionality to an object, at first it semms like we can do the same with inheritance, let's see this example, we have this diagram:
InputStream <- FileInputStream

1. We want to have a BufferedInputStream that buffers characters before reading
- Inheritance
InputStream <- FileInputStream <- BufferedFileInputStream
But also for InputStream alone
InputStream <- BufferedInputStream

- Decoration
```java
in = FileInputStream()
in = BufferedInputStream(in)
```

Here, we can see that using inheritance we should add 2 more classes whereas we only need one using decoration but it's not that much of a diffreence

2. We want to have a GZIPInputStream that decodes the input as GZIP
- Inheritance
InputStream <- FileInputStream <- BufferedInputStream <- GZIPBufferedFileInputStream
But also without Bufered
InputStream <- FileInputStream <- GZIPFileInputStream
But also without File
InputStream <- GZIPInputStream


So we have to write three classes

- Decoration
```java
in = FileInputStream()
in = BufferedInputStream(in)
in = GZIPInputStream(in)
```

3. Now, what happens if we want to add another implementation
- Inheritance
InputStream <- FileInputStream <- BufferedInputStream <- GZIPBufferedInputStream <- GZIPBufferedFileCountInputStream
But also without GZIP
InputStream <- FileInputStream <- BufferedInputStream <- BufferdFileCountInputStream
But also without Buffered
InputStream <- FileInputStream <- FileCountInputStream
But also without File
InputStream <- CountInputStream


And now we have to add 4 classes
- Decoration
```java
in = FileInputStream()
in = BufferedInputStream(in)
in = GZIPInputStream(in)
in = LineCountInputStream(in)
```
 
You get it, it gows expentionally and usually we don't implement all the cases because there is just so much of them. Decorators is the way to go in those cases

## Bridge
the purpouse of a Bridge is to isolate two subsystem so that we can modify them independently
**Note:** The diffrence between design patters is the intent

Unlike adapters and decorators bridges are doing much work, it has a lot of code

Example: JDBC (Java Database Connector, maybe!), as long as I use the interfaces,
I can swap the db implementations (sqlite, mysql, ...) changing the client code.

**Wrap up: The intent of*
- Adapter: mix an interface into a class that we don't have the code for and that is not currently using that interface
- Decorator: change the way that a method behaves
- Bridge: isolate a subsystem

There is absolutely no reason to not mixing them together

## Facade
It's a false front, it's like a facade you see when you entre a bank: just the banker with the cache machine and some PCs but back door there is much more than that

The purpouse is to simplify, it provides a simplifying inteface to another interface, it's not trying to isolate subsystems from each other.

## 
