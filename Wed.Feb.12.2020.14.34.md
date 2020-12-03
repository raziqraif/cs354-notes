# Isolation/Protection VI

To get lab 1 grades, `cat /homes/cs354/grades/hart111/lab1.rpt`

Never allow user-modifiable data to affect the kernel.

When you write kernel code, don't access user data. But if you do, you have to be extremely careful.

EFLAGS:
	IF - Interrupt flag:
		0 - interrupts disabled
		1 - interrupts enabled
	
	IOPL - IO Protection Check first 2 bits of CS (CPL) if CPL <= IOPL, IO is allowed
		- Basically checks that you're at least *this* privileged to do IO


Fundamental principle - all computation must be done by a process. No execution can be done by the OS itself
No execution can occur outside a process. At any time, SOME process must be running.

In Xinu this is process 0 nulluser()

