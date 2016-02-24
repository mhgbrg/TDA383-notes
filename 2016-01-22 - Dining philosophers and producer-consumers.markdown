# Dining philosophers and producer-consumers

N philosophers sit at a round table with N chopsticks. A philosopher needs two chopsticks to eat.

The naive solution would be to wait for one chopstick to be free, take it, then wait for the other one to be free. This could result in a deadlock, meaning that every philosopher has only one chopstick, and everyone is waiting for a free chopstick.

## Timeout

You could introduce timeouts so that the philosophers only wait for a certain set time before putting down their chopsticks. This could introduce a livelock if all philosophers run simultaneously. 

## Break the symmetry

*Textbook solution*

Decide that some of the philosophers start by taking the chopstick to the right, and some start by taking the chopstick to the left. For example only the first one can be different. This will prevent any deadlocks from happening. 

How to prove **mutex**? Use invariants. When a philosopher is eating he holds bolds the chopsticks.

How to prove **no deadlock**? Deadlock would occur if all philosophers have one and only one chopstick. This can't happen since the ones who get the left chopstick first and the ones who get the right chopstick first can't both have one chopstick at the same time.

How to prove **no starvation**? We assume that an eating philosopher stops eating at some point, and that the semaphores are fair (meaning that permits are given out according to some sort of queue system).

## Limit the usage of resources

Use a general sempahore to represent N-1 available chairs. This semaphore has N-1 permits.

## Fair semaphores in Java

Default constructor for `Semaphore` is not fair. However, you can set a flag when creating a semaphore that tells it to be fair.

## Preventing deadlocks

* Order of resources. Only allow resources to be acquired in a certain order (solution 1).
* Break the chain. Identify circular waiting chains and don't allow enough processes to "fill" the chain (solution 2).

## Producer-consumer

*Producers* produce to a buffer, *consumers* consume from the buffer.

### Unbounded buffer

*Infinite bar*

Producer produces at any rate, consumer consumes at any rate. The only time that there is waiting is when the consumer is faster than the producer. This allows the producer to work freely.

**Can overflow buffer if consumer is too slow!**

### Bounded buffer

*Real life*

If buffer is full -> producer has to wait
If buffer is empty -> consumer must wait

This is exactly like an unbounded buffer, only that the producer doesn't overproduce.

### One-slot buffer

Producers only produce if buffer is empty, consumers only consume if buffer is full. Use two semaphores to indicate whether or not the buffer is full or empty. The producers need to wait for `empty` to be released, and consumers need to wait for `full` to be released. *See slides for code*

### N-sized buffer

Is represented as a line in memory, but can be reasoned with as a circle. You have two pointers, `head` and `tail` for keeping track of the buffer. The producer adds to the tail and the consumer pulls from the head. You allocate `S` spots (the length of the buffer), and when you reach the end of the buffer you just loop around to the beginning (% S).

The first solution works when you have a single consumer and producer. If you have more the buffer can be overwritten by simultaneous reads and writes. *See slides for code*

To solve this you introduce another semaphore for both producing and consuming, so that only one consumer can consume at the same time, and only one producer can produce at the same time. *See slides for code*

## Readers-writers problem

*Read and write from database*. You might want to allow several actors to read from the database simultaneously, as long as no one is writing to it. Writing can only be done by one actor at the same time.

Using two locks can result in starvation for writers. To prevent this you alternate reads and writes so that everyone has a chance to do work.