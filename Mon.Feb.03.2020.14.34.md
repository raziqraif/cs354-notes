# Isolation / Protection II

Needs hardware support:
- Instruction set - When a cpu architect comes up with a design and tells the software designers what instructions they can use. Instructions are thought of in two ways:
	- Privileged
	- Non-Privileged
- Need user mode / kernel mode
- Trap:
	- User Mode->Kernel Mode
	- Kernel Mode<-User Mode
-Memory Protection - Check if the process can look into an area it is trying to access


32 Bit has EFLAGS
64 Bit has RFLAGS

We are using x86 so two fields of EFLAGS are super important:
- IF Flag (10th bit) if set to 1 means interrupts are enabled, else disabled
- IDP


CLI instruction - Clear interrupts (set 10th bit of IF flag to 0)
STI instruction - Start interrupts (set 10th bit of IF flag to 1)

If we're on a linux pc and the only instruction in main.S is CLi, the instruction will disable interrupts for the whole system.

If any process that's running can run the CLI instruction it not only impacts the running process but the rest of the system. So these instrs should only be executed in kernel mode.

EFLAGS is shared between all processes (it's a CPU state), 

Each CPU will have its own EFLAGS, so some can run with interrupts enabled and some disabled.

Real mode in the CPU mostly exists for backward compatibility. This exists because intel supports legacy software (to a fault lol).

Real mode switches to protected mode by using register CR0. CR0's first bit CR0[0] starts off as 0, which indicates real mode. This bit is changed to 1 by using a mov instruction during bootstrapping. mov is not a privileged instruction, we use it all the time. Here, the instruction is not the only factor in whether an action is privileged, the place being accessed matters as well. 

Instead of calling something privileged instruction, it is more accurate to say "privileged operation", for example placing a value into register CR0, because that affects the behavior of the whole system.

Loading the value of the IDTR should NOT be done in user mode, so that's another example of a privileged operations. There is an LIDT instruction that is privileged for doing so.


x86-specific features: Old style peculiar architecture where if we have RAM we can support relocatable code

```mermaid
graph LR;
	C-->GCC;
	GCC-->Binary;
```
```
+----------------------+
|                      |                                                   
+----------------------+   +-------------+                                 
|    PID 1000          | < | STACK       |                                    
+----------------------+   +-------------+                                 
|                      |   | ...         |                                 
+----------------------+   +-------------+                                 
|                      |   | HEAP        |                                 
+----------------------+   +-------------+                                 
|                      |   | ...         |                                 
+----------------------+   +-------------+                                 
|                      |
+----------------------+
```

This process is called segmentation: for the program to reside at an indeterminate place, we have to define a beginning (base) and an end (limit). That way we can reference the base in terms of rebasing the binary.


Nowadays people don't like segmentation. Now we use paging, which turns out to be more efficient, scalable, etc. So we just define the base for each segment as 0 and the limit as 0xffffffff... If we are accessing a segment beyond our base and limit we get a segmentation fault. Though x86 has segmentation built in to their hardware, setting the base to 0 and limit to max memory lets us use paging on backwards hardware. This way we can eliminate segmentation faulting due to actual segmentation, segmentation is effectively not used.

There are registers:
- CS - Code Segment - First two bits indicate privilege level (0-3). Kernel is 0, user mode is 3. 2 and 1 aren't used by 1.
- DS - Data Segment
- SS - Stack Segment