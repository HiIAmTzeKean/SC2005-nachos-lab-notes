---
tags:
  - 🌱
  - OS
  - ComputerScience 
date: 07--Nov--2022
---

# Lab 4

TLB miss flow
Translate virtual address → TLB search → not found → to IPT → found page → update TLB → return translated address

TLB miss + Page fault
Translate virtual address → TLB search → not found → exception → IPT → page fault → exception → pageInPageOut → update TLB → return translated address

Whenever there is an issue with translation, it starts with a TLB miss as TLB is the first level of check.

---
Links: 