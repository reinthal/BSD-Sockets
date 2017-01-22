# BSD-Sockets-2

This server uses the system unix call ```select(2)``` in order to concurrently accept connections from clients.

# How to build the server and client

Simply use 
```shell
-bash-4.1$ g++ server-concurrent.cpp -o sconcurrent
TODO
-bash-4.1$ g++ client-simple.cpp -o sclient
-bash-4.1$ g++ client-multi.cpp -o mclient
```
# Running the comparison
If you compare the output of the concurrent server with the non-concurrent one from BSD-sockets-1 you see a significant improvement!

First, setup the iterative server


```shell
-bash-4.1$./server-iterative
Attempting to bind to port 5703
Socket is bound to 0.0.0.0 : 5703
```
Then run the benchmark like this

```shell
user@comp$
-bash-4.1$ ./cmulti.out localhost 5703 100 100 "cellar door"
Simulating 100 clients.
Establishing 100 connections...
  successfully initiated 100 connection attempts!
Connect timing results for 100 successful connections
  - min time: 3.864045 ms
  - max time: 7000.980814 ms
  - average time: 2621.380895 ms
 (0 connections failed!)
Roundtrip timing results for 100 connections for 100 round trips
  - min time: 9.979731 ms
  - max time: 3038.689249 ms
  - average time: 388.355631 ms
    ```
Observe that 65 clients were dropped. Now we setup the concurrent server
```shell
user@comp$./server-concurrent
Attempting to bind to port 5703
Socket is bound to 0.0.0.0 : 5703
```
and run the same test

# TODO
  
