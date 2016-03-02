# Data Ductus, reactive objects

## Classical programs

Classical programs were essentially "computations". You provide input data to your program and get a result in the end. The program can be presented with a flow chart.

## Modern programs

Modern programs doesn't work like this. Nowadays you get *evolving input* and produce *evolving output*. Think for example a reactive webpage. You press a button and maybe a color changes - this is an example of evolving input and output.

## Time-triggered systems

Data is read from the *environment* by the program at predefined times. Concurrency is needed here because the environment is concurrent. A lot of stuff can happen at the same time, so you need a program that can handle this.

Each new input here is handled by a classical program. Input is read, processed and output is generated. The difference is that it is now happening continously.

## Event-triggered systems

Here the environment is the one responsible for telling the program that there has been a change. These changes are called *events*. The program **reacts** to events from the environment. In order to handle concurrency in this model you can either buffer up the events or run multiple event handlers in parallell. These parallell event handlers can buffer up events internally as well.

Each event is handled by a classical program in this case as well. The change in these systems compared to the old classical programs is that each tiny event is viewed as a classical program, but the entire system is viewed as something else.

## Event loops vs reactive programming

The traditional way of handling events is with an event loop. The main program blocks until an event happens, and then the event is parsed and handled by a function. For example when listening to keyboard events a keyboard event handler in the operating systems get and IRQ and then sends an event to the main program (which is blocking until such an event occurs). The event is then sent to a handling function that takes care of the event.

Even though the main program is blocking, the keyboard event handler and the event handling function are both reactive. We take a reactive call, turn it to an active call, and then make it to a reactive call again. Why do we need to do this?

Enter reactive programming! The main idea here is that the event handling function can get it's event directly from the operating system, effectively cutting out all middle hands. This makes for a system that is very simple to reason about, as pretty much all the overhead of event handling is removed.

The event handlers can then in turn trigger new events, and thus you can have large *event chains*.

## Timber programming model

Timber is a programming language that focuses on this style of programming. Every event handler is an object that has methods and state. The methods in these event handlers are **mutually exclusive**, so each event handler is essentially a monitor.

The event handlers can have two different kinds of functions - actions and requests. Actions are async, and will not block the caller. The execution will continue in its own thread until it is done. Requests on the other hand block the caller until the method has finished executing. These types of methods are used for example when returning values.
