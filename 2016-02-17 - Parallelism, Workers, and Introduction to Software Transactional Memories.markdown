# Parallelism, Workers, and Introduction to Software Transactional Memories

Processors are not getting faster, but they are getting more cores. In order for programs to get faster you must utilize these cores.

## Parallelizing computations

When using associative operations (like `+`) you can split up expressions into parts that can be easily parallelized.

    1 + 2 + 3 + 4 = (1 + 2) + (3 + 4) = 3 + 7 = 10

## Workers

*Workers* are simple processes that are only responsible for starting computations. Instead of starting one process per computation you start a number of worker threads that continuously do the computations.

The reason for using workers is that there is a sweet spot of how many simultaneous processes you can have and still have an efficient program. If you have too few, you might not use the system to its full potential. If you have too many, you get an overloaded system. Threads are expensive to start and handle, so you don't want too many!

You usualy put these workers in a *worker pool*. When you want to do some computation you tell the worker pool to execute it. If there is a worker available it will be executed instantly. If all workers are busy the task will be put in a queue for later execution.

Ideally you have a system wide worker pool that is handled by the operating system. The operating system can then be the one responsible for maxing out the system, which makes sense. Sometimes application-wide and component-wide pools are also useful.

### Active and passive workers

A worker can be in one of two states - *active* and *passive*. Active workers are workers that are performing tasks, while passive workers are workers that are waiting to be assigned a task. A task delegated to a worker pool will not be executed until there is a passive worker that can take on the task.

## Locks

There are many potential problems when using locks to handle concurrent programming (deadlock, starvation, etc). 

## Software Transactional Memory (STM)

This type of concurrency is used in transactions in databases. A transaction consists of a number of operations that should be executed atomically. The way it works is that each variable has a *version number* that changes whenever the variable is changed. If the version number for a variable you care about has changed since after you began your transaction (and you weren't the one to change it), the transaction cannot finish. If there are no other changes, the transaction can be **committed**. If there are changes, the transaction is **rollbacked**.

This type of concurrency is called *optimistic concurrency*. More than one process can be in the critical section at the same time, and nothing will happen until they step on each others toes (changes the value of the same variable). If something goes wrong in the transaction (the value of a variable has changed) it is simply rerun until it succeeds.

Transactions are easy to compose (and easy to reason about), and prevent deadlocks from happening.

There are drawbacks of using transactions. They don't guarantee fairness - a large transaction can be starved by many small ones. You also need to keep track of all the version numbers to be able to tell if a variable has changed inside a transaction. This can be very expensive (it uses a lot of memory).

 