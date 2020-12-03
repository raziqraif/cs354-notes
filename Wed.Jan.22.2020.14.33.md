First lab comes out Friday Morning

Function calls are use in both apps and operating systems

The downside of function calls is they are slow and have overhead. The benefit is seen as outweighing the cost. The drawback is addressed by the hardware developers in terms of design choices. They provide support to speed up function calls.

GCC lays out the binary file so that we have different sections:

```
+-----------+
| .stack    | --------------\
+-----------+                |
|           |                |
+-----------+                |
|           |                |
+-----------+                |
| .heap     | <- dynamic mem \
+-----------+                 |
| .data     | <- global data  \
+-----------+                  >- These are the image in RAM when the program is loaded
| .text     | <- program code /
+-----------+
```

Ex: if there are two instances of a process P with PIDs 5000 and 7000:

Memory may look like:

```
+--------+
| 5000   | -> Points to something like the above 
+--------+
|        |
+--------+
|        |
+--------+
| 7000   |
+--------+
|        |
+--------+
|        |
+--------+
|        |
+--------+
```

Ex:
```
int abc(void) {
	int x, y, z; <- these are local variables (would be on the stack) in C but as park points out, there are no such thing as local variables in memory, it's just memory.
	x = 5;
	y = 7;
	z = def(x, y);
}
```

Ex2:
```
int x, y, z;
int abc(void) {
	x = 5;
	y = 7;
	z = def(x, y);
}
```

In this example, x, y, z are all global variables and would be in the data segment

Park then proceeds to talk about calling convention and how instruction passing works. He talked very high level about cdecl.


