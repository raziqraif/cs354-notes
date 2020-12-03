# Scheduling wrap-up

We discussed the solaris version data structure called the multilevel feedback queue. This data structure gives us constant overhead.

In XINU we have linear overhead (Insert costs linear search). A balanced tree structure or heap gets logarithmic time. We want constant time for scheduling, however, for a real OS. 

In Solaris, our array of structs has 60 elements. Each has two pointers, a pointer to the beginning of a linked list and a pointer to the end. The array index is the priority of the process. 

Insert has constant overhead: go to the index of the process' priority (constant), and add it to the back of that list (constant). 

Dequeue has Loop down the list to our priority (constant, up to 60), then get the first element (constant, just get the first one).

We cannot use this technique for CFS. We have to monitor every process' CPU usage, we can't bound it between 0 and 60. Therefore we must use a balanced search tree, which has log insert/extract, which is the best we can do.

Solaris example (link on site to dispatch table). If we create a new process and it used its entire time slice, it is marked as CPU bound (if it didn't use its own time slice, it's IO bound). If it's a CPU bound process, we demote its priority, but it gets a larger time slice. 

CPU bound process eats up entire time slice. IO bound process does not. This is used as the classifier but obviously this is not a perfect metric. Processes will move up and down in priority based on what they did last.

# Inter-Process Communication

"XINU is special, it is only useful as a real operating system in extremely specific embedded system contexts"

We want to ensure atomicity (ensure messages are not garbled up with others). This requires a limit on the max size of a message. Kernel assures programmer that messages smaller than this will be atomic. 
