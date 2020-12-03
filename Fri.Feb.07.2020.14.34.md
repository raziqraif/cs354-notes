# Isolation / Protection IV

GDT contains CS, DS, SS in Linux

In XINU, the GDTR holds:

| CS | 11 |
| --- | --- |
| DS | 11 |
| CS | 00 |
| DS | 00 |

This is different from Linux, because the system runs in protected mode, not in user/kernel mode. 

You could optimize xinu to only use CS and DS since DS ~= SS and both contain non-code.


With hardware support in lab 2, we will implement trapping but we are not going all the way. We don't need to know te details of GDT but we will modify the setprio system call so it has the structure of how linux/unix/windows etc work using hardware.

IDT: in RAM we set up the data structure IDT and point IDTR to the beginning of the IDT using the kernel mode instruction load idt. There are 256 interrupts supported by the architecture. The first 32 interrupts from 0-31 (each row is an 8 byte data structure), are reserved by x86 for exceptions/faults. These are also all synchronous interrupts. INT 0 -> div 0, INT 13 -> general protection fault, INT 32 -> hardware clock, rings once per millisecond. 

Hardware clock configured with clkinit()

We are going to use a system call to switch from kernel to user mode and vice versa

We need to create a wrapper function like fork(). Somewhere in fork(), which is just regular C code, is a trap instruction. With hardware support we will use that trap instruction to transition to kernel mode. 

There are 2 versions of executing a trap: the old way (which we'll use) and the new way (which is more complex).

Traditional approach is the int instruction, which we execute with an immediate of our interrupt number. 


In linux, the syscall interrupt is 0x80. This causes the hardware to use IDTR to go to 0x80 in the IDT, then jumps to the interrupt handler for 0x80. Instead in XINU we will put the syscall handler @ 33 


@ that point we will add a system cal dispatcher. This is the "gateway" that we branch off to our system call. We will only have to modify 2 syscalls into true trap system calls. So we will write the code for _Xtrap33:

So essentially we make 2 wrapper functions for the system calls getpid and getprio which will now be considered "internal" kernel functions. We make the interrupt handler which takes the syscall number in eax and go to the right function.

Return an error if there is no system call for the number. True syscalls must be written carefully so that the kernel doesn't crash with invalid values.


Wrapper syscall functions also need to pass argument (specifically for getprio, which has one arg) in ebx/ecx/edx etc. Return values are in eax after syscall.


the only difference between this and a true syscall is that we don't start in user mode. Because XINU, we start in kernel mode and stay there, we just simulate the process.

