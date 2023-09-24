---
tags: 🌱
date: 01--Oct--2022
---

# Nachos

Written in C++

- Uses a hardware simulation (virtual machine)
- Run as a single UNIX process on Host OS

![[Pasted image 20221001184023.png]]

## Why threads and not [[Process]]

Nachos provides only threads such that all threads share the same code and global variables.

Each thread has a context block, like a [[Process control block|PCB]] which stores the context and register information. Nachos does not maintain a process table but information on threads are maintained as private data in Thread object. Thus, unlike a centralised storage of PCBs, Nachos has its thread table entries scattered, and a pointer is needed to obtain a thread information.

## [[Context switch]]

A context switch involves suspending current thread, saving its state and restoring state of incoming thread.

1. Save state of current registers in oldThread
2. Address that PC pointing to should be the address following the call to switch()
    1. It should be like continuing a code execution after a normal function call
    2. So instead of saving current PC, return address for switch stored instead
3. Once oldThread is saved, load all registers of the newThread
4. Once all registers loaded for newThread, finally load in the PC for new thread
    1. Note that PC must be loaded last, since once PC is loaded, the next instruction to be done is to execute the code in the newThread
    2. Thus context switch is said to be done once PC of newThread is loaded in

---
Links:
