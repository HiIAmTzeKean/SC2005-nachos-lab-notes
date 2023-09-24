---
tags: 🌱
date: 01--Oct--2022
---

# Scheduler

Maintains a ready queue of ready [[Thread-Nachos]]
- Ready queue is [[First come first serve|FCFS]]
    - Threads appended to back of queue and popped from front
- All threads are also assumed to have same priority

The scheduler is [[Non-preemptive scheduling]].
- Scheduler only invoked when thread voluntarily gives up CPU

![[Pasted image 20221002132802.png]]

## Functions
- ReadyToRun
    - Set thread state to [[Ready state]]
    - Add thread to the ready queue
- FindNextToRun
    - Gets the thread at head of queue
- Run
    - Check if there is any overflow from stack in old thread
    - Set new thread to be [[Running state]]
    - Context switch to new thread via SWITCH
    - Terminate old thread if thread finished

---
Links: 