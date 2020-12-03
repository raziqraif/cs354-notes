We mostly just care about the system calls in order to accomplish things. The OS provides a system call API or system call interface.

Given:
```
main() {
	printf(); // Nothing special happens, just a library call
}
```
But inside printf():
```
printf() {
	write(fd, buf, count); // This is a system call, which goes down to the OS and asks it to send the buffer to the device (or file descriptor)
}
```

trap, syscall, supervisor call are all the same.

In xinu putc() is a system call.

In linux, this:

```
main() {
putc(CONSOLE, 'h');
putc(CONSOLE, 'i');
putc(CONSOLE, '\n');
}
```

Is equiv to:
```
main() {
write(stdout, 'h', sizeof(char));
write(stdout, 'i', sizeof(char));
write(stdout, '\n', sizeof(char));
}

or I guess the way park does it:
char a = 'h';
write(stdout, a, sizeof(char));
a = 'i';
write(stdout, a, sizeof(char));
a = '\n';
write(stdout, a, sizeof(char));

```
OS is only involved in the loading process of running code.

Scheduler switches contexts from one process to another.

Upon process creation, processes have special status: user mode status.

If all it does is simple stuff, it stays in user mode. When the program makes a system call, its status changes from user mode to kernel mode.

"kernel" is used to distinguish between android and the linux it runs on top of and such

Kernel mode allows actions that affect the hardware directly, not allowed in user mode. Kernel is trusted so that attackers cannot hijack a syscall to enter kernel mode and execute arbitrary code.


After making a syscall we return from the syscall function and re-enter user mode.