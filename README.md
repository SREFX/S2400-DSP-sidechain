# S2400-DSP-sidechain
lv2 plugins for cross-bus sidechaining on the Isla S2400 DSP card.

### S2400-sidechain-lv2.zip
The send and receive plugins compiled in lv2 format. Just place this in the folder on the SD card and sync as normal.

### /S2400-sidechain
The send plugin, this contains all of the envelope shaping controls. Built using the Distrho framework.

### /S2400-sidechain-recv
The receive plugin, this is dumb with no parameters and simply receives the envelope from the send.

### How cross-bus works
We are using POSIX commands to define a chunk of static RAM of a few bytes (2 * float per potential stereo envelope signal) in /dev/shm/. This chunk is defined in both the send and receive plugins. The send then pushes the envelope blindly to that chunk and the receive listens blindly to that chunk.

This means that you can stack as many receive plugins as you like, they will all happily listen to the same chunk.

Both the send and receive plugins have bus selection - A, B, C, D to allow for multiple Send instances. Set a send to bus B and any receives set to bus B will receive the sidechain envelope.

You can find the code for this in the main .cpp/hpp files for each plugin. It's framework and format agnostic so easily protable I think.

### To-Do
Move envelope controls to the receive, allowing different envelope attack, release, depth etc. at each destination?

### known bug
--changing a receive's bus to one with no input signal while sidechaining is being applied will leave the receives level at the last known envelope value. Either reload or swap back to a bus with an envelope for now. Doing the to-do above will make the fix for this cleaner I think.
