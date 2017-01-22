# BSD-Sockets-2

This server uses the system unix call ```select(2)``` in order to concurrently accept connections from clients.

# How to build the server and client

Simply use 
```shell
user@comp$ g++ server-concurrent.cpp -o server
user@comp$ g++ client-simple.cpp -o sclient
user@comp$ g++ client-multi.cpp -o mclient
```
and then run the client and server in separate terminals
```shell 
user@comp$./server-concurrent
Socket is bound to 0.0.0.0 5703
```
then connect using client
```shell
user@comp$./client localhost 5703
```
# Running the comparison
If you compare the output of the concurrent server with the non-concurrent one from BSD-sockets-1 you see a significant improvement!

First, setup the iterative server


```shell
user@comp$./server-iterative
Attempting to bind to port 5703
Socket is bound to 0.0.0.0 : 5703
```
Then run the benchmark like this

```shell
user@comp$./cmulti.out localhost 5703 100 100 "hello world"
Simulating 100 clients.
Establishing 100 connections...
  successfully initiated 100 connection attempts!
Connect timing results for 35 successful connections
  - min time: 1.172856 ms
  - max time: 305.349450 ms
  - average time: 120.638499 ms
 (65 connections failed!)
Roundtrip timing results for 28 connections for 100 round trips
  - min time: 10.759337 ms
  - max time: 92.500083 ms
  - average time: 46.819618 ms
    ```
Observe that 65 clients were dropped. Now we setup the concurrent server
```shell
user@comp$./server-concurrent
Attempting to bind to port 5703
Socket is bound to 0.0.0.0 : 5703
```
and run the same test

# TODO
  
