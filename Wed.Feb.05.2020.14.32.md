# Isolation / Protection III

We have rings: 0,1,2,3

0:00 <- kernel mode
1:01
2:10
3:11 <- user mode

Corresponding to 

How do we mark data kernel or user

Data has its own segmentation, the DS register, which also has a privilege data which corresponds to the above

Same for stack using SS. We don't use these registers for memory protection but we do use them for isolation/protection so we know who is accessing data belonging to whom

In lab 2 we'll use the system call getprio() which simply returns the priority of a process

All system calls start by disabling interrupts

At the end, the enable interrupts
