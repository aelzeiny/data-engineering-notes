# DIY Node.js

### Prereq
* I'm assuming you know how JavaScript works: Single-Threaded, Async, the EventLoop, and the Event Queue

### Node Internals
* NodeJS is made of 3 different components:
    * A threading lib: LibUV
    * A JS Engine: V8
    * Some C++ code
* V8, SpiderMoney, and JavaScript Core are all JS engines
* global vars like console.log, setInterval, and setTimeout must be provided by NodeJS; not V8. They are written in C++, and our JS code interops with C++ via V8.
* NodeJS is like an interpretter, but it parses bits of JS and executes it in V8. It extends V8, but does not actually parse JS code.
* V8 has a default implementation of an eventloop, and different implementors can extend it.

### Shallow Dive into LibUV
* [Source](https://levelup.gitconnected.com/nodejs-runtime-environment-libuv-library-event-loop-thread-pool-5f5ecadc0318)
* Node uses libuc in order to do async threading in C.
    * Async funcs
    * Threads
    * Timers
    * Child Processes
    * Event Loops
* JS may be single-threaded, but Node's main thread should not be blocked by JS execution. 
* The main thread offloads jobs to libuv, and libuv assigns the task to a worker in its threadpool. When tasks are done, they sync-up back to the main thread.

### Tutorial
* follow along [here](https://www.youtube.com/watch?v=ynNDmp7hBdo).

### How is Dino different from Node
* The foundational talk that launched Dino was the [founder expressing regrets](https://www.youtube.com/watch?v=M3BM9TB-8yA).
* For starters, Rust instead of C++. Tokio instead of LibUV (mostly for Rust compatibility.)
* Security: JS is a clean, fast (-er than python), and SANDBOX'd dynamic lang. Node breaks this by giving programs access to any/all OS calls.
* The Deno process has 1 entry & rebound point into V8 via protobufs. Makes the internal lbrary cleaner.
* Package management a bit different.