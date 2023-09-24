---
tags: 🌱
date: 02--Oct--2022
---

# Interrupt handling

Has a PendingInterrupt class which maintains a priority queue of interrupts

## Functions
- Schedule
    - Insert a new interrupt into pending list
    - Inserts interrupt in sorted order based on timer expire
    - [[Timer]] also calls schedule to create timer interrupts
- OneTick
    - Function that process timer tick
    - Updates *totalTicks* which is a global variable
    - Disable interrupt first, because interrupt handlers run with interrupts disabled
    - Invokes CheckIfDue to check if there is an interrupt that must be triggered and run the interrupt handler. This will repeat till no more interrupts are due
    - Finally enable interrupts again so that we can proceed with running threads
    - If [[Context switch]] is due, OS will allow context switch now that OS code is done
- CheckIfDue
    - The function checks if the interrupt at head of queue is to be handled
    - If the timer interrupt for CPU is due, the Timer function Timer:TimerExpired will be called
- YieldOnReturn
    - Called by Timer:TimerInterruptHandler() which is only invoked when the timer interrupt for CPU has expired
    - YieldOnReturn sets variable yieldOnReturn to true → Interrupt:OneTick will force a [[Context switch]]

---
Links: 