He got a bunch of questions on lab, so a clarification:
 - Problem 3 (and therefore 4)
 - (1, 2a, 2b) -> reched() -> ctxsw()
 - Only works if the new process has been context switched out (evicted).
 - If new process is newly created (create(), suspended, resume):
    - Create needs to push stuff into the stack so that the new process doesn't look like it was newly created (looks like it was evicted)

 - problem 3.3 & 3.5
   - Measure CPU Usage.
     - Timestamp after ctx switch in, timestamp after ctx switch out
     - Details need to consider above corner case.
   - Code in resched will never be reached by a new process

When program runs for the first time, make sure to take the timestamp, it can't be in reched/ctxswitch since jumping directly to main. One trick is to put a call in the epilogue function. When ctx switch runs, instead of returning to main, return to a take-the-start-timestamp function, then return to main after that.

This is a "detour" -> "Hackers use it all the time" Bascially rop.

-----

kprintf is not reliable. Store into globals, then print after interrupts are enabled.

-----

Be super careful when embedding asm because lots of things can go wrong. And hopefully that happened to you. If you got lucky and nothing went wrong, that's bad.
"Always be wary of gcc" "gcc causes bugs"

With the long long type taking up two registers, potential issues.

Use local for asm, then copy between local and global outside of the inline assembly. This can protect you from gcc.

-----

You're creating extra process table fields to measure usage. prrawcpu type thing in ms. That number replaces the priority field. That feild is already there though. Don't break existing code with prprio, use a new feild.

*Don't delete old priority field.*

Resume returns priority, don't break resume.

"I'm not asking you to create production code"

All of his tests timeout at 8 seconds. So they should fit into a short. Yes, the uint32 or whatever type it is will overflow, but for now you can ignore it.

"This guy will overflow; ignore it"

"Consider all the parts of XINU you need to change"
 - This includes getprio type things
 - insert to change orrder of comparison
 - other places priority is used

-----

A few more words were added to the lab document in 3.4 reiterating what he said.

########################################################################
# Lecture
########################################################################

      | CPU-Bound | IO Bound
----------------------------------
prio  | low       | high
---------------------------------
slice | high      | low

Linux scrapped that approach a bit ago, and now uses fair scheduling, like we're doing with XINU.
CFS => Completely Fair Scheduling

Logorithmic overhead - needs to pick guy w/ least cpu usage. 'minimum' operation
A balanced tree / heap is used for CFS in XINU.

Modern OSes use O(1) constant overhead for process scheduling.
Solaris: multi-level priority queue (Feedback Queue)

priorities 0-59 <- we're only focused on these guys
priorities 60-99 system

(Linux only supported 40, Windows 16 priority levels)

0-59: Time Share
 0: low
 59: high

 Array of structs w/ 60 entries, each entry has two pointer: to the beginning and end of a  FIFO linked list

|---|
| 59   [ ]->[ ]->[ ]
|---|
| . |
| . |
| . |
|---|
| 0   [ ]->[ ]
|---|

dequeue: remove table[0].first
enqueue: update table[0].last

Linuxes current scheduling implementation: "It is not something that someone who studied scheduling would implement" "In fact, it was created by a phisician"