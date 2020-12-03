354 Midterm March 9 In-Class

CPU has an Instruction set, Registers

Registers -> fastest memory
Cache -> Next fastest
RAM -> Main memory

Galileo is x86_64, has 64-bit data bus

We won't worry about HD/SSD until we reach filesystems

Can't execute software instructions from SSD/HDD, can't talk directly to CPU except in specific cases

We can copy code from a disk into memory and then execute it
Turing created first Universal computer, computer that can load programs into memory as well as data. 

Batch processing: a person using a machine, then another person using a machine, etc. Common in supercomputers.

Next method was:
+-------+
|usr 1  |
+-------+
|data 1 |
+-------+
|usr 2  |
+-------+
|data 2 |
+-------+
  ...

So that multiple users can be on at once, we want scheduling to make a CPU able to run more than one process at any given time.

Processes need to be identified:
PIDs - unique identifier for each process


Context switch:
save off the state of a process, load the state of another. Run the other, then save that processes state.

Function calls grew in 70s and 80s instead of using GOTO. Championed by Dijkstra. 

With this advent, processors started supporting stack frames.

