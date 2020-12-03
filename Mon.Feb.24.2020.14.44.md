# Context switching

Park continued to talk about the code in ctxsw.S. I'm not gonna write it down because I know how it works.

There is a corner case. We assume that old was running beforehand when ctxsw is called. Because processes in xinu are created by create() have not run yet, it could happen that a newly created process that has never run is at the front of the ready list. So we make it look like created processes called ctxsw in the past, so we push return info, ebp, eflags, and general purpose regs onto the stack of freshly created processes.