# V8 Engine

Nowadays there are various type of ecosystems available, let it be for Python, Java, Node.js.

If we specifically talk about Node.js there are a lot big of platforms and softwares using Node.js like JioCinema, Uber, etc. And we are going to hear the Node.js a lot of time, if we are mainly going to involved in JS-based backend systems/ecosystems or backend in general.

The term Node.js is relevant to understand most of the modern JS stuff, for example if we take a modern library like React, for React to work in development we need Node.js on our system.

- What is Node.js
- What it is not
- Internals of Node.js
- How Node.js code is written
- Chrome V8 engine
- Internals of V8 engine

## What Is Node.js

Nowadays it is pretty common knowledge that it is a runtime environment.

If you go through the Node.js web page. There is a simple definition: "Free, OSS, Cross Platform JS Runtime"

But let's take a deeper dive and understand the runtime environment part.

### What Is a Runtime Environment

You might have seen a newborn babies, so what happens with newborn babies is that their parents give them the most suitable environment, feed them good quality food, shelter, etc. The parents ensure to give the baby a habitable environment so that the baby can grow and start living. That is what a habitable environment is in human terms.

Now let's understand the habitable environment with respect to technology, for you to do something specific in a particular technology, maybe the technology is not sufficient enough and you want to add more capabilities to the said technology. That is what a runtime environment is. Runtime environment is a software, it's nothing crazy, it can provide extra ecosystem stuff that the technology lacks inherently and enhance the capability of the corresponding technology. We are talking about JS as the technology here.

JS is a programming language. It has a lot of limitations, JS as a programming language is modern, high level and has some crazy feature but as a programming language it is not powerful enough to make end-to-end applications, scripts anything that modern JS is able to do. Inherently it does not have all of those capabilities, JS is a simple and plain programming language altogether. What runtime environment does is, provides extra capabilities to JS.

What extra capabilities, you might have seen function like `setTimeout` and `setInterval`, their use case is to access timers. JS in itself doesn't know how to access timers. They are not inherent part of JS, there is no such logic implemented in itself, you can check the ECMAScript spec. So how do they work? These are some extra capabilities provided by the runtime environment. And these capabilities can vary according to the runtimes. Runtime knows a lot of interesting things like accessing timers, one of the most simple runtime environments is your browser, browser can act as a runtime environment for JS. They provide capabilities like reading and modifying HTML, accessing timers, DOM-related functions. JS is about loops, functions, etc.

Browsers provide networking capabilities as well.

In a nutshell we have a runtime environment which is a software and inside it we have our plain JS running and this runtime provide the extra capabilities to JS. For a major period the browser was the only runtime environment for JS.

In 2009 Ryan Dahl, thought what if we take JS out of the browser and make a new runtime environment in which we run JS outside of the browser and give it capabilities of the OS. He released a runtime in which JS was running and the capabilities were different from the browser runtime. It gave capabilities like read/write files on disk, access processes. Timer-related capabilities, making network calls. with the help of the OS APIs

The networking capabilities were enhanced. In browsers the networking capability was in a way that you can make the request to the outside world. In Dahl's version it was not just able to make external requests, it also made possible that other machines can communicate with your machine.

This new environment was Node.js. This made possible to use JS outside the frontend world and start building technology using JS outside the browser. And be able to access machine resources due to which the server-side development stuff are being done in JS because of Node.js.

### Cross Platform

In computer science platform defines the architecture we are working on. Windows as an OS is different as Mac as an OS. If we write code in C++, it is a non-cross-platform language. You write code it is saved in a file (program) we convert it to a executable piece of code and then we run that file, if we create the executable on Windows we won't be able to run it on Mac. But there are technologies like Java, Python and JS that are cross-platform. Write once and run it everywhere. How they achieve cross-platform achievement is different from each other.

### Internals of Node

Node provides a lot of different capabilities including some libraries, it gives access to something called V8, libuv library it gives access to the event loop and gives access to a lot of OS level stuff.

but how are they given access to ? 

the Node ecosystem internally has all of these capabilities but outside we write gets access to a lot of interesting functions like setTimeout, what Node does is, exposes this set of functions to the JS layer and this JS layer sometimes has the whole implementation in itself and sometimes interacts with the C++ layer. a majority of the Node runtime is written in C++. 

setTimeout(callback, 0)

in Node if you pass 0 ms as an setTimeout value , it is automatically converted to 1 ms. the time in ms if undefined it is automatically converted to 1 ms. if it is not undefined it is converted to a number. if your timeout value is not in the range from 1 to the maximum value , so if you pass 0 it is converted to 1

This behavior is runtime to runtime specific

but why use C++ along with JS in the runtime? C++ is blazingly fast , JS is not apart from that a lot of capabilities is easy to interact with lot of OS capabilities through C++ as it is very fast and low level. that is why JS layer needs to interact with C++ layer. 

if you want to interact directly with the OS using JS , then the OS has to expose those capabilities to the JS layer.

the most important question is how is it C++ not cross platform but Node is when it is primarily written in C++ and that is where V8 comes in.

if Node and browser both are runtimes how do they communicate with each other ??

they communicate similarly how two processes communicate with each other. 

- inter-process communication: we do not use it as the processes can be on different machines, as we deploy the server and client differently
- networking communication

## Internals of V8

V8 is the JS engine , what is a JS engine ? in a nutshell JS engine has all the logic written to take a JS code and run that on your machine, this logic is technically written in a JS engine. and V8 is one of the engines alongside with Chakra(legacy), SpiderMonkey and JavaScriptCore. 

why different JS engines ?
they optimize on different stuff , browsers run on a high end machines like our day to day devices, because if you see there are machines with extremely low capabilities like IoT devices, in IoT devices if you have to code in JS you cannot get the similar behavior we get in high end machines, so there the JS conversion and running JS needs to be as light as possible, so there are different optimized JS engines for different devices and use cases

V8 engine is developed by Google and powers the Chrome browser and majorly written in C++. interestingly it is a different component altogether and not tightly coupled with the browser or the Node runtime. it is not the case that Node exists that is why V8 exists. 

what v8 has that makes it capable of running code in machine ?

there a lot of components of V8 engine , and they together make possible running the code on the machine

- parser
- interpreter (Ignition)
- compiler (TurboFan) earlier it was Crankshaft , ( Sparkplug is a baseline compiler, Maglev is a mid-tier optimizing compiler, and TurboFan targets peak optimization.)

parser

parser is used by most of the programming languages. it is used to take our code and tokenizes it and creates something called as an AST(abstract syntax tree), it is very common step in programming languages

it takes let x = 1;

and it takes the individual tokens and it creates a tree out of it, this tree is technically the complete representation of our code

but why ? to make it understandable for the remaining components of the V8 engine and make it compatible for the later analysis and optimization

interpreter (Ignition)

Ignition converts the AST to bytecode. what is bytecode ? it is an intermediate code and not the final code or the code we write. It is more compact and portable, creation of bytecode makes it cross platform capable. 

Ignition was not present in the early versions of Node

earlier Node only had a compiler called Crankshaft and it used to again and again parse the code and convert it to AST, later Ignition was introduced and it created bytecode and the bytecode was intermediate code it ran faster than the JS code. it keeps converting the code to bytecode and starts running it

why do you need TurboFan when Ignition already does the job ?
that's where Node and V8 comes in , V8 collects some information at runtime, like there might be some function that is being called frequently, Ignition creates a short bytecode (unoptimized version) and once it is run V8 collects runtime information the V8 optimizes the code

how ? using TurboFan which is an optimization compiler, it is designed to make effective highly optimized machine code, if some line executing frequently it can cache that (the feedback about the object shapes/maps and value types), you pass an object to a function, how exactly it is passed and the value is going to take , it is going to take a type corresponding type to that, repeatedly passing same type of object will create a cached version of it to avoid recomputing

but the optimization is based on past runs of the code, whatever amount of code Ignition has executed based on that metric tries to create a machine code that the engine can directly use instead of the bytecode, the machine code is created based on past executions. 

function f({name= ""}) {}

for this function it will try to create an internal representation on the type of the object, it will create a single type representation but maybe after some run we pass different type of an object, the TurboFan will fall back to the previous unoptimized version of the code (bytecode) or it may resume in another tier.

why is it hard to write C++ ? because you're the one who decides the variable type but in JS the language decides it and it comes with a performance cost.

all of this is happening in runtime that is why we call it JIT

there are two types of compilation
- just in time (JIT)
- ahead of time (AOT)

what we do in C++ is AOT , compile before running the code
JIT is done during the runtime, compilation and running together.

so TurboFan optimizes the hot part of the code

apart from the capabilities of compiler, parser, etc it has more things like the memory aspect, access to the call stack and heap

interestingly it also does garbage collection
it uses Orinoco for it

Orinoco is GC, it does young generation and old generation segregation

if there is an object , it does a couple of GC cycles, it will GC unnecessary object , if some object is still in use, it will keep it in the old generation, because it is still in use and will be used again soon, the variables we create newly are young generation and they are more likely to be garbage collected sooner(because there is a good chance it maybe a temporary object)

so orinoco segregates memory in two parts: young generation and old generation

the old memory objects stay for a longer period of time

and most of this happens concurrently to the running code

it also does memory compaction, if a lot of fragments are created because something was present in that memory block but now it is vanished/gc'd , it tries to do memory compaction to free up space

it also sweeps in chunks

in TS when we write, the code is written as typed code but it still converted to JS and then executed

This whole process of parsing, interpreting, and compilation is JIT 
