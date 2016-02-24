# Debugging

Debugging concurrent programs is generally really hard, but a few methods can be used for doing it more effeciently.

## Reasoning backwards

Instead of starting from the beggining and going forward, you start from a certain state and step backwards.

For example you can look at a log file and find the bad state, and from there backtrack the execution using the log until you find the faulty code.

Hypothesis: *this happened* or *that happened*.

## Things to do while debugging

A small program is easier to debug than a large program. Make your program smaller! Or begin debugging when the program is small.

