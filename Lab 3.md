---
tags:
  - 🌱
  - OS
  - ComputerScience 
date: 07--Nov--2022
---

# Lab 3

Thread operations and synchronisation

Each thread starts in JUST_CREATED state.

There are only 3 was for thread to be put to READY
- Thread::ReadyToRun (Blocked to Ready)
- Thread::Yield (Running to Ready called)
- Thread::ReadyToRun (JUST_CREATED to Ready called in Fork)

[[Semaphore]] in Nachos
- Non-negative
- P() == wait()
    - If value is 0, put process to sleep
- V() == signal()
    - FIFO queue. First thread in queue is put on ready

Lock
Keep tracks of who is the owner of the thread, and only owner can release lock unlike semaphore.
- Acquire()
    - Lock becomes busy and all other threads blocked except owner
- Release()


---
Links: 