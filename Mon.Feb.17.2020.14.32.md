For lab 2, make sure that when we write code we understand what we are doing and write comments about it.

For example:

1. Wrapper Function
2. System call dispatcher
3. Existing (Legacy) Internal Kernel Function, for example getprio()

We do not edit #3's Assembly, because it is created with GCC. When we pass arguments to the Legacy Internal Kernel Function, we have to pass the arguments correctly as CDECL expects.


# Scheduling

INT instruction does a lot of CPU cycles. Instead of INT we can use an optimized instruction like sysenter and sysexit. 

In the context of XINU, the scheduler's function is called resched(). This function reschedules processes. The first way to access it is sleepms(ms) which sleeps for ms milliseconds. Next is a clock interrupt. Interrupt 32 is handled by the clock (in clkdisp.S) which calls the clock handler (clkhandler.c). resched() decides with a check whether to keep running or reschedule the program. The scheduler then decides by looking at the ready list. If the current one has a higher priority after running, it keeps running. Otherwise it is scheduled out.o

clock handler will iret back to before the interrupt.


Suppose we sleepms(500). After 500 clock ticks, the process needs to be woken up. It is kept in a sleep list which is maintained so each clock interrupt, we tick down the sleep list. When something reaches zero (the front member) we take it out of the sleep list and put it into the ready list. The clock handler handles this. 

In operating systems we want constant overhead, NOT linear. Linear = bad. Log is okay if constant isn't doable. 

Keep track of CLI. We want to do it when we enter kernel mode in the dispatcher.