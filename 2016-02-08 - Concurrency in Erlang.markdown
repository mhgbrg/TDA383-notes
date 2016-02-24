# Concurrency in Erlang

## Processes

Processes in Erlang are completely separate beings that can only communicate by sending and receiving messages. They have no shared data!

Each process has its own heap that no other process has access to. This is not space efficient, because each message has to be copied between two processes heaps. A good thing though is that each process can do garbage collection independently without interfering with other processes.

## Messages

Receiving a message is a blocking operation. If there are no messages in the mailbox the operation will block until a message is found.

**Will the operation block until any message is found, or if a message that matches a pattern is found?** - It will block until a message that matches the pattern is found.

To kill a process in the shell, use `ctrl-G`.

The receive statement *leaks* variables.

The oldest message will be tried against every pattern until it matches. If it doesn't match, we do the same thing for the second message, and the one after that, and the one after that etc.

## Registered processes, distributed environments

**Summarize!**

## Exceptions 

**Summarize!**

## List comprehensions

List comprehensions are used to generate lists. They are formatted in sort of the same way as the mathematical expression to describe sets.

    > [X + 2 || X <- [1, 2, 3], X > 1].
    [4, 5]

The expression can contain generators (`X <- [1, 2, 3]`), expressions (`X + 2`) and filters (`X > 3`).

## Lambda expressions

Lambda expressions are used to create anonymous functions inline in the code.

    spawn(fun() -> io:format("new process created!~n") end).

This allows you to use local variables in the function body, for example:

    X = [1, 2, 3],
    spawn(fun() -> io:format("X = ~p~n", [X]) end).

This is not possible when using a normal function.

## Higher order functions

## Begin-end blocks

Probably not used very much, but good to know about.

**Summarize!**