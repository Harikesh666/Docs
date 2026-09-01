# V8 Engine

Nowadays there are various type of ecosystems available, let it be for Python, Java, Node.js.

If we specifically talk about Node.js there are a lot big of platforms and softwares using Node.js like JioCinema, Uber, etc. And we are going to hear the Node.js a lot of time, if we are mainly going to involved in JS-based backend systems/ecosystems or backend in general.

The term Node.js is relevant to understand most of the modern JS stuff, for example if we take a modern library like React, for React to work we need Node.js on our system.

- What is Node.js
- What it is not
- Internals of Node.js
- How Node.js code is written
- Chrome V8 engine
- Internals of V8 engine

## What Is Node.js

Nowadays it is pretty common knowledge that it is a runtime env.

If you go through the Node.js web page. There is a simple def: "Free, OSS, Cross Platform JS Runtime"

But let's take a deeper dive and understand the runtime env part.

### What Is a Runtime Env

You might have seen a newborn babies, so what happens with newborn babies is that their parents give them the most suitable environment, feed them good quality food, shelter, etc. The parents ensure to give the baby a habitable env so that the baby can grow and start living. That is what a habitable env is in human terms.

Now let's understand the habitable env with respect to tech, for you to do something specific in a particular tech, maybe the tech is not sufficient enough and you want to add more capabilities to the said tech. That is what a runtime env is. Runtime env is a software, it's nothing crazy, it can provide extra ecosystem stuff that the tech lacks inherently and enhance the capability of the corresponding tech. We are talking about JS as the tech here.

JS is a programming lang. It has a lot of limitations, JS as a programming lang is modern, high level and has some crazy feature but as a prog lang it is not powerful enough to make end-to-end applications, scripts anything that modern JS is able to do. Inherently it does not have all of those capabilities, JS is a simple and plain prog lang altogether. What runtime env does is, provides extra capabilities to JS.

What extra capabilities, you might have seen func like `setTimeout` and `setInterval`, their use case is to access timers. JS in itself doesn't know how to access timers. They are not inherent part of JS, there is no such logic implemented in itself, you can check the ECMAScript spec. So how do they work? These are some extra capabilities provided by the runtime env. And these capabilities can vary according to the runtimes. Runtime knows a lot of interesting things like accessing timers, one of the most simple runtime envs is your browser, browser can act as a runtime env for JS. They provide caps like reading and modifying HTML, accessing timers, DOM-related functions. JS is about loops, functions, etc.

Browsers provide networking capabilities as well.

In a nutshell we have a runtime env which is a software and inside it we have our plain JS running and this runtime provide the extra capabilities to JS. For a major period the browser was the only runtime env for JS.

In 2009 Ryan Dahl, thought what if we take JS out of the browser and make a new runtime env in which we run JS outside of the browser and give it capabilities of the OS. He released a runtime in which JS was running and the capabilities were different from the browser runtime. It gave capabilities like read/write files on disk, access processes running on the RAM. Timer-related capabilities, making network calls.

The networking capabilities were enhanced. In browsers the networking capability was in a way that you can make the request to the outside world. In Dahl's version it was not just able to make external requests, it also made possible that other machines can communicate with your machine.

This new env was Node.js. This made possible to use JS outside the frontend world and start building tech using JS outside the browser. And be able to access machine resources due to which the server-side dev stuff are being done in JS because of Node.js.

### Cross Platform

In CS platform defines the architecture we are working on. Windows as an OS is different as Mac as an OS. If we write code in C++, it is a non-cross-platform lang. You write code it is saved in a file (program) we convert it to a executable piece of code and then we run that file, if we create the executable on Windows we won't be able to run it on Mac. But there are tech like Java, Python and JS that are cross-platform. Write once and run it everywhere. How they achieve cross-platform achievement is different from each other.

### internals of node

node provides a lot of diff capabs including some libraries, it gives acces to something called v8, libuv lib, it gives access to the event loop and gives access to a lot of os level stuff.

but how are they given access to ? 

the node ecosystem internally has all of these capabilites but outside we write gets access to a lot of interesting funcs like setTimeout, what node does is, exposes this set of funcs to the js layer and this js layer sometimes has the whole implementation in itself and sometimes interacts with the cpp layer. a majority of the node runtime is written in cpp. 

setTimeout(cb, 0)

in node if you pass 0 ms as an settimeout value , it is automatically converted to 1 ms. the time in ms if undefined it is automatically converted to 1 ms. if it is not undefined it is converted to a number. if your timeout value is not in the range from 1 to the maximum value , so if you pass 0 it is converted to 1

This behavior is runtime to runtime specific

but why use cpp along with js in the runtime? cpp is blazingly fast , js is not apart from that a lot of capabilities is easy to interact with lot of os capabitilies through cpp as it is very fast and low level. that is why js layer needs to interact with cpp layer. 

if you want to interact directly with the os using js , then the os has to expose those capabilities to the js layer.

the most imp ques is how is it cpp not cross platform but node is when it is primarilly written in cpp and that is where v8 comes in.

if node and browser both are runtimes how do they commmunicate with each other, 

they communicate similarly how two processes communicate with each other. 

- inter-process communication: we do not use it as the processes can be on diff machines, as we deploy the server and client differently
- networking communication

## internals of v8

