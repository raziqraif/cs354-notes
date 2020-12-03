Because XINU always picks the process with the highest priority, until the scheduler is implemented a process with higher priority will always run first. So if an infinite loop process has priority 20 a process with priority 40 will never run.

If there is a tie in priority, the kernel will sequentially run the processes in the proctab as a round-robin sequence. 