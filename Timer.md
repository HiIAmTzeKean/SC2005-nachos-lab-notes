---
tags: 🌱
date: 02--Oct--2022
---

# Timer

## Functions
- TimerInterruptHandler
    - OS will call this function each time when the timer expires to handle a timer interrupt
    - Function is called with interrupts disabled
- TimerExpired
    - Function to call the interrupt handler
    - Invoked when the timer for thread expires
- TimeOfNextInterrupt
    - Returns the next interrupt tick

---
Links: 