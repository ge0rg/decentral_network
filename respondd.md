# Respondd vs babel vs efficiency
 
The current form of the plugin concept doesn't allow long running socket connections because from the plugin there is no access to the event loop. 
The current babel plugin works, however it induces quite some load on the node when deployed on large networks this is because for its function it'll have to 
connect to babeld, dump everything babeld knows, filter through what is required and 
rework that to the json response structure.

babeld supports a monitor command which means that changes are communicated whenever thy happen in babeld. monitor also allows subscribing to specific events 
only.

It should be possible to keep state across plugin invocations however when 
opening a socket and processing its data whenever the plugin executes. While 
reducing CPU load a lot, during high message load, data might be lost when 
buffers are full but not read from.

To fix this, the plugin must be able to register a handler in the main loop of 
respondd and respondd must be able to monitor multiple file descriptors as part 
of that event loop.

What are the alternatives?
