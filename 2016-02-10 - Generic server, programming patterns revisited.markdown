# Generic server, programming patterns revisited

A generic Erlang server consists of two parts:

1. A process that listens for requests and returns replies.
2. Code that sends requests to the server process. This is kind of like an interface to users of the server.

There is a module called `genserver` that abstracts away all the generic server stuff that is present in all servers. This implementation allows you to just send in a function that specifies a case of all the functionality of the server.

