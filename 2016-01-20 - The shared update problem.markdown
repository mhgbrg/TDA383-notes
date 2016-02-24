# The shared update problem

## Questions from before

Atomic == operation that is executed without interruption

## Semaphore

Object with two operations. `wait()` and `signal()`. Obs! Named differently in Java - `acquire()` and `release()`.

Oldest synchronization technique.

Q: Are semaphores handing out permits by a queue-based order?
A: The original definition of semaphores has no guarantee of order, but there are implementations that do this.

Both `wait` and `signal` has to be *atomic*.

### How to convince yourself that the counter is now correct?

Any execution is OK, as long as it gives us the right answer. If an execution yields the number 200 000 it is correct, otherwise it is not. 

Create a sequential program that executes the atomic operations of each thread in a certain order. If you can find such an order, the program is hopefully correct. If you can't find it, it isn't.

## Limited critical reference

Statement = line of code.

Statement has a critical reference if it writes to or reads from a shared variable (a variable that can be read or modified by another thread). *Limited Critical Reference* is satisfied when a statement has at most one critical reference.

Try to write code where each statement satisfies Limited Critical Rection, for example by using temporary variables: `c = tmp + 1` instead of `c = c + 1`.

## `await` statement

`await(E)` - waits until expression E becomes true. Can be implemented using *busy waiting*: `while (!E);`

**Don't do this!**

## Implementing a semaphore using `await`

**Mutual Exclusion Problem**

*See slides for code*

**Mutex** - At most one thread at a time is in its critical section.
**No deadlock/livelock** - If both processes attempt to enter their critical section, one will succeeed.
**No starvation** - A process attempting to enter its critical section will eventually succeed.

### Showing the mutual exclusion property

Create an execution state graph - a graph of all states (sort of like a Markov Chain). To do this you start from the beginning of the execution and simply follows all possible states of execution. This can be represented as a matrix.

Draw a matrix of all possible states and color all reachable states. A state is reachable if there is a sequence of arrows that gets to it from the initial state. There will still be arrows from unreachable states to unreachable states, and from unreachable states to reachable states. But they don't matter.

You have deadlocks if you have states that can't go to any other states, but not having states like these does not guarantee not having any deadlocks.

For there to be no starvation there has to be at least one path from the await-line to the critical section.

## Labs

Labs 1 and 3 involves a lot of coding, labs 2 and 4 are more about figuring out what to do.

## Invariants

*A condition that holds for all reachable states (a boolean expression)*

You declare expressions that will always be true - these are the invariants. You then define the state that you want to prove can't happen, and look at all states that lead to that state. You then use the invariants to prove that none of these states will ever occur.

    invariant 1: t1:4..8 <=> flag[0] = true
    invariant 2: t2:4..8 <=> flag[1] = true
    
    expression for invalid state: ~(t1:7 && t2:7)
    
    invalid state: t1:7, t2:7, flag[0], flag[1] - we know the value of flag[0] and flag[0] because of the invariants

## Lock

*Binary semaphore*

The `synchronized` keyword in Java. When used in the method declaration it means that the entire method is a critical section. You can also declare custom synchronized blocks.

    private int counter = 0;
    
    public void increment() {
        synchronized (this) {
            count++;
        }
    }

When using `synchronized` the lock is automatically released even if an exception occurs and the method exits before it is finished. This can be fixed by releasing the semaphore in a `finally` block.

