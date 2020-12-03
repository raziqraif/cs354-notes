Runtime storage for xinu processes:

Typical machine:

| text   |
|---|
| data |
| bss |
| heap |
| ... |
| ... |
| stack |

This program layout exists somewhere in memory, and there is a copy for duplicate of the same process at a different location in RAM.

Instead of this, Xinu looks like this:
XINU processes all share a single text segment, single data segment, and single bss segment

| text |
|---|
| data |
| bss | 
| heap |
| ... |
| ... |
| Process 3 Stack |
| Process 2 Stack |
| Process 1 Stack |

Effectively all these processes are threads (especially since all processes in xinu are just function calls)

This is okay because Xinu is not a multi user system. Instead, we will focus on the absolute core items that modern operating systems implement so we will not create a runtime environment with library linking and such. 

Xinu is 32 bit

Xinu diagram (stolen from ammar):
```
+---------+         +------------+
| Galileo | Serial  |    Xinu    |
|  Board  <--------->   Server   |
|         |         |            |
+---------+         +-----^------+
                          |  Network
      +-------------------|-------+
      |             +-----v----+  |
      |             |cs-console|  |
      |             |          |  |
      |             +----------+  |
      |      Xinu Workstation     |
      +---------------------------+
```

The bootloader places the system in protected mode after executing "real mode" code during the bootstrapping process. 

After the bootstrapping process, xinu is loaded into memory with the layout:

| text |
|---|
| data |
| bss | 
| heap |
| ... |
| ... |
| nulluser() |

After nulluser() runs, we may have more processes:

| text |
|---|
| data |
| bss | 
| heap |
| ... |
| ... |
| mykprintf() |
| main() |
| start() |
| nulluser() |