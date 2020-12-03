# Scheduling

We know the processing modes:
1. Batch
2. Round Robin

Function calls are slow but the tradeoff is worth it.

From fast to slow:
- No function calls
- Function calls
- System calls
- Context switching
- Context switching with virtual memory


Apps can be:
- IO Bound
- CPU Bound

| --- | Cpu-bound | I/O Bound |
| --- | ---  | --- |
| Priority | LOW | HIGH |
| QUANTUM | LARGE | SMALL |

Unix, Linux, Windows all use the above.

Linux now uses FAIR scheduling.

Linus has a lot of rants about this. When linux followed typical scheduling, it had one difference. It used a large time slice for I/O bound processes and small for CPU-bound

| --- | Cpu-bound | I/O Bound |
| --- | ---  | --- |
| Priority | LOW | HIGH |
| QUANTUM | SMALL | LARGE (800ms) |

Linux was first seriously adopted by server farms. This added significant latency. 
If we know for SURE that a process is IO bound, the large time slice is okay. However if we look like a process that is IO bound and then change behavior, the process will run for the full time slice and monopolize the CPU. 

Fair scheduling: for each process, monitor how much CPU each process has used. We pick the process that has the least CPU time used. This is a load balancing approach. 

Why is this not the standard? 
- Issue 1. If P1 is created at 0 and P2 is created at 30 mins, P2 will get priority over P1 for a long time. To fix this, we make it look like new processes are using CPU already, otherwise it would not run.
- Issue 2. If a process has slept for a long time, it can gain a large amount of time.

Fair scheduling is log(n), not constant. 