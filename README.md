# BSD-Sockets

This lab demonstrates the usage of Berkely sockets

https://en.wikipedia.org/wiki/Berkeley_sockets

an API for communication between IP-networked unix computers. It also demonstrates how concurrently handled connections improves response time of the server.

# How to build the server and client

Simply use 
```shell
-bash-4.1$ cd server/
-bash-4.1$ g++ server-concurrent.cpp -o sconcurrent.out
-bash-4.1$ g++ server-iterative.cpp -o iserver.out
-bash-4.1$ cd ../client/
-bash-4.1$ g++ client-simple.cpp -o sclient.out
-bash-4.1$ g++ client-multi.cpp -o mclient.out
```
# Running the comparison
If you compare the output of the concurrent server with the iterative you see a significant improvement!

First, setup the iterative server


```shell
-bash-4.1$ cd ../server/
-bash-4.1$./iserver.out
Attempting to bind to port 5703
Socket is bound to 0.0.0.0 : 5703
```
Then run the benchmark like this

```shell

-bash-4.1$ ./mclient.out localhost 5703 100 100 "cellar door"
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

Now we setup the concurrent server

```shell
-bash-4.1$./sconcurrent.out
Attempting to bind to port 5703
Socket is bound to 0.0.0.0 : 5703
```
... and run the same test ...
```shell
and run the same test
-bash-4.1$ ./mclient.out localhost 5703 100 100 "hello world"
Simulating 100 clients.
Establishing 100 connections...
  successfully initiated 100 connection attempts!
Connect timing results for 100 successful connections
  - min time: 4.641029 ms
  - max time: 11.023767 ms
  - average time: 8.137233 ms
 (0 connections failed!)
Roundtrip timing results for 100 connections for 100 round trips
  - min time: 186.746056 ms
  - max time: 238.715438 ms
  - average time: 220.088684 ms

```
When we compare the average connection time for clients we have a significant improvement, 388 ms vs 8 ms. but the roundtrip time for the echoes did not improve as drastically! This is explained by the blocking server loop in the iterative server
```cpp
while( processFurther )
		{	
		
			
			while( processFurther && connData.state == eConnStateReceiving )
				processFurther = process_client_recv( connData );
		
			while( processFurther && connData.state == eConnStateSending )
				processFurther = process_client_send( connData );
		
		}
```
# Round Trip Times: Concurrent vs Iterative
The iterative server had a 388 ms rtt and the concurrent server had a 220 rtt. This is not a significant improvement. I'm not sure why this is but my guess is that the server is still only working with one thread.
  
