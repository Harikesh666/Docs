# V8 Engine

Nowadays, there are various types of ecosystems available, whether for Python, Java, or Node.js.

If we specifically talk about Node.js, there are many large platforms and software systems using Node.js, such as JioCinema, Uber, etc. And we are going to hear about Node.js many times if we are mainly going to be involved in JS-based backend systems/ecosystems or backend development in general.

Understanding Node.js is relevant to understanding most modern JS stuff. For example, if we take a modern library like React, we need Node.js on our system for React to work in development.

- What is Node.js
- What it is not
- Internals of Node.js
- How Node.js code is written
- Chrome V8 engine
- Internals of V8 engine

## What Is Node.js

Nowadays it is pretty common knowledge that it is a runtime environment.

If you visit the Node.js website, you will find a simple definition: "Free, OSS, Cross Platform JS Runtime"

But let's take a deeper dive and understand the runtime environment part.

### What Is a Runtime Environment

You might have seen newborn babies. What happens with newborn babies is that their parents give them the most suitable environment and provide them with good-quality food, shelter, etc. The parents ensure that they give the baby a habitable environment so that the baby can grow and start living. That is what a habitable environment is in human terms.

Now let's understand the habitable environment with respect to technology. For you to do something specific in a particular technology, maybe the technology is not sufficient enough, and you want to add more capabilities to the said technology. That is what a runtime environment is. A runtime environment is software; it's nothing crazy. It can provide extra ecosystem stuff that the technology inherently lacks and enhance the capability of the corresponding technology. We are talking about JS as the technology here.

JS is a programming language. It has a lot of limitations. JS, as a programming language, is modern and high-level and has some crazy features, but as a programming language, it is not powerful enough to make end-to-end applications, scripts, or anything that modern JS is able to do. Inherently, it does not have all of those capabilities. JS is a simple and plain programming language altogether. What a runtime environment does is provide extra capabilities to JS.

What extra capabilities? You might have seen functions like `setTimeout` and `setInterval`; their use cases are to access timers. JS in itself doesn't know how to access timers. They are not an inherent part of JS; there is no such logic implemented in JS itself. You can check the ECMAScript spec. So how do they work? These are some extra capabilities provided by the runtime environment. And these capabilities can vary according to the runtimes. A runtime knows a lot of interesting things, like accessing timers. One of the simplest runtime environments is your browser. A browser can act as a runtime environment for JS. It provides capabilities like reading and modifying HTML, accessing timers, and using DOM-related functions. JS is about loops, functions, etc.

Browsers provide networking capabilities as well.

In a nutshell, we have a runtime environment, which is software, and inside it, we have our plain JS running. This runtime provides extra capabilities to JS. For a major period, the browser was the only runtime environment for JS.

In 2009, Ryan Dahl thought, “What if we take JS out of the browser, make a new runtime environment in which we run JS outside of the browser, and give it capabilities of the OS?” He released a runtime in which JS was running, and the capabilities were different from those of the browser runtime. It provided capabilities like reading/writing files on disk, accessing processes running on RAM, using timers, and making network calls with the help of OS APIs.

The networking capabilities were enhanced. In browsers, the networking capability was such that you could make a request to the outside world. Dahl's version was not only able to make external requests, but it also made it possible for other machines to communicate with your machine.

This new environment was Node.js. This made it possible to use JS outside the frontend world and start building technology using JS outside the browser. It also made it possible to access machine resources, due to which server-side development work is being done in JS because of Node.js.

### Cross Platform

In computer science, a platform defines the architecture we are working on. Windows as an OS is different from Mac as an OS. If we write code in C++, it is a non-cross-platform language. You write code, and it is saved in a file (a program). We convert it into an executable piece of code, and then we run that file. If we create the executable on Windows, we won't be able to run it on Mac. But there are technologies like Java, Python, and JS that are cross-platform. Write once and run it everywhere. How they achieve cross-platform compatibility differs from one another.

### Internals of Node

Node provides a lot of different capabilities, including some libraries. It gives access to something called V8 and the libuv library. It gives access to the event loop and a lot of OS-level stuff.

But how is access to these capabilities provided?

The Node ecosystem internally has all of these capabilities, but the code we write gets access to many interesting functions like setTimeout. What Node does is expose this set of functions to the JS layer. This JS layer sometimes has the whole implementation in itself and sometimes interacts with the C++ layer. A majority of the Node runtime is written in C++.

```js
setTimeout(callback, 0)
```

In Node, if you pass 0 ms as a setTimeout value, it is automatically converted to 1 ms. If the time in ms is undefined, it is automatically converted to 1 ms. If it is not undefined, it is converted to a number. If your timeout value is not in the range from 1 to the maximum value, it is converted to 1. For example, if you pass 0, it is converted to 1.

This behavior is runtime-specific.

But why use C++ along with JS in the runtime? C++ is blazingly fast, whereas JS is not. Apart from that, it is easy to interact with many OS capabilities through C++, as it is very fast and low-level. That is why the JS layer needs to interact with the C++ layer.

If you want to interact directly with the OS using JS, then the OS has to expose those capabilities to the JS layer.

The most important question is: How is C++ not cross-platform while Node is, when it is primarily written in C++? That is where V8 comes in.

If Node and a browser are both runtimes, how do they communicate with each other?

They communicate similarly to how two processes communicate with each other.

- Inter-process communication: we do not use it, as the processes can be on different machines because we deploy the server and client differently.
- Networking communication

## Internals of V8

V8 is the JS engine. What is a JS engine? In a nutshell, a JS engine has all the logic written to take JS code and run it on your machine. This logic is technically written in a JS engine. V8 is one of the engines alongside Chakra (legacy), SpiderMonkey, and JavaScriptCore.

### Why are there different JS engines?

They optimize for different things. Browsers run on high-end machines like our day-to-day devices. However, there are also machines with extremely low capabilities, such as IoT devices. If you have to code in JS for IoT devices, you cannot get similar behavior to what we get on high-end machines. Therefore, JS conversion and execution need to be as lightweight as possible. This is why there are different optimized JS engines for different devices and use cases.

The V8 engine is developed by Google, powers the Chrome browser, and is primarily written in C++. Interestingly, it is a different component altogether and is not tightly coupled with the browser or the Node runtime. V8 does not exist because Node exists.

### What does V8 have that makes it capable of running code on a machine?

There are a lot of components in the V8 engine, and together, they make it possible to run the code on the machine.

- parser
- interpreter (Ignition)
- compiler (TurboFan); earlier, it was Crankshaft (Sparkplug is a baseline compiler, Maglev is a mid-tier optimizing compiler, and TurboFan targets peak optimization.)

### Parser

A parser is used by most programming languages. It is used to take our code, tokenize it, and create something called an AST (abstract syntax tree). It is a very common step in programming languages.

It takes `let x = 1;`.

It takes the individual tokens and creates a tree out of them. This tree is technically the complete representation of our code.

But why? It makes the code understandable to the remaining components of the V8 engine and makes it compatible with later analysis and optimization.

### Interpreter (Ignition)

Ignition converts the AST to bytecode. What is bytecode? It is intermediate code, not the final code or the code we write. It is more compact and portable; the creation of bytecode makes it cross-platform capable.

Ignition was not present in the early versions of Node.

Earlier, Node only had a compiler called Crankshaft, and it used to parse the code again and again and convert it to an AST. Later, Ignition was introduced, and it created bytecode. The bytecode was intermediate code, and it ran faster than the JS code. It keeps converting the code to bytecode and starts running it.

### Why do you need TurboFan when Ignition already does the job?

That's where Node and V8 come in. V8 collects some information at runtime. For example, there might be some function that is being called frequently. Ignition creates a short bytecode sequence (an unoptimized version), and once it is run, V8 collects runtime information and optimizes the code.

How? By using TurboFan, which is an optimization compiler. It is designed to make effective, highly optimized machine code. If some line is executing frequently, it can cache that (the feedback about the object shapes/maps and value types). You pass an object to a function. The way it is passed and the value it takes correspond to a particular type. Repeatedly passing the same type of object will create a cached version of it to avoid recomputing.

But the optimization is based on past runs of the code. Based on the amount of code Ignition has executed, V8 uses that metric to try to create machine code that the engine can directly use instead of bytecode. The machine code is created based on past executions.

```js
function f({name= ""}) {}
```

For this function, it will try to create an internal representation of the type of the object. It will create a single type representation, but maybe after some runs, we pass a different type of object. TurboFan will fall back to the previous unoptimized version of the code (bytecode), or it may resume in another tier.

Why is it hard to write C++? Because you're the one who decides the variable type, but in JS, the language decides it, and it comes with a performance cost.

All of this is happening at runtime, which is why we call it JIT.

### There are two types of compilation

- just in time (JIT)
- ahead of time (AOT)

What we do in C++ is AOT: we compile before running the code.
JIT is done at runtime; compilation and execution happen together.

So TurboFan optimizes the hot part of the code.

Apart from the capabilities of the compiler, parser, etc., it has more things, such as the memory aspect and access to the call stack and the heap.

### Interestingly, it also does garbage collection

It uses Orinoco for it.

Orinoco is a GC; it performs young-generation and old-generation segregation.

If there is an object, it goes through a couple of GC cycles. It will garbage-collect unnecessary objects. If an object is still in use, it will keep it in the old generation because it is still in use and will be used again soon. The newly created variables are in the young generation, and they are more likely to be garbage-collected sooner (because there is a good chance that an object may be temporary).

So Orinoco segregates memory into two parts: the young generation and the old generation.

The old memory objects stay for a longer period of time.

Most of this happens in parallel with the running code.

It also does memory compaction. If a lot of fragments are created because something was present in that memory block but has now vanished/been GC'd, it tries to perform memory compaction to free up space.

It also sweeps in chunks.

### In TS, when we write code, the code is typed, but it is still converted to JS and then executed

This whole process of parsing, interpreting, and compilation is JIT.
