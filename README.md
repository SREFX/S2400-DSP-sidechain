# S2400-DSP-sidechain
lv2 plugins for cross-bus sidechaining on the Isla S2400 DSP card.

### S2400-sidechain-lv2.zip
The send and receive plugins compiled in lv2 format. Just place this in the folder on the SD card and sync as normal.

### /S2400-sidechain
The send plugin, this contains all of the envelope shaping controls. Built using the Distrho framework.

### /S2400-sidechain-recv
The receive plugin, this is dumb with no parameters and simply receives the envelope from the send.

### How cross-bus works
We are using POSIX commands to define a chunk of static RAM of a few bytes (2 * float) in /dev/shm/. This chunk is defined in both the send and receive plugins. The send then pushes the envelope blindly to that chunk and the receive listens blindly to that chunk.

This means that you can stack as many receive plugins as you like, they will all listen to the envelope. However if you try to place multiple send plugins they will all blindly spam the same chunk of RAM.

You can find the code for this in the main .cpp files for each plugin. It's framework and format agnostic so easily protable I think.

### To-Do
Move envelope controls to the receive, allowing different envelope attack, release, depth etc. at each destination.

Get both send and receive to define 4 x chunks of RAM, create selection param in both plugins. This allows multiple instances. e.g. Send 1 set to chunk A, send 2 set to chunk B, recv 1 set to chunk B, recv 2 set to chunk A.
