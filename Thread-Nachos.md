---
tags: 🌱
date: 01--Oct--2022
---

# Threads

## States
Note that there is no [[Terminated state]] unlike typical OS since being terminated is just the deletion of the thread, and thus there is no need for an enum to store such a state
- Ready
    - Thread is on ready list but another thread is currently running
- Running
    - Thread is running and there is a pointer currentThread which points to the running thread
- Blocked
    - Thread is waiting for some event. Happens when thread calls Thread:Sleep()
- Just_created
    - Status is assigned as default parameter when a new thread object is created
    - Thread does not have a stack, and thus must wait for Thread:Fork() to be called before it can run

## Functions
- Fork
    - Allocates stack to new process for SWITCH call
    - Initialise registers
    - Invokes [[Scheduler]]:ReadyToRun → Set thread state to read and place thread in ready queue
- Yield
    - Find the next thread to run using [[Scheduler]]:FindNextToRun
    - If there is another thread, we call [[Scheduler]]:ReadyToRun to put old thread into back ready queue and set status to [[Ready state]]
    - Then call [[Scheduler]]:Run on the current thread
    - Note that this is preemption call, where a thread that is currently running is forced to relinquish CPU
- Sleep
    - Set status to blocked
    - Find next to run
    - If there exist a thread, run the next thread
    - Else call Interrupt:Idle to wait for the next interrupt
- Finish
    - Called once the thread is done
    - Invokes [[Thread-Nachos]]:Sleep first, then destroyed later
    - Thread not immediately destroyed since we are still using the stack. Ptr to thread is saved, and the next time [[Scheduler]]:Run is called, it will check if there is a thread to destroy
    - This ensures thread not destroyed when we are on stack

---
Links: 