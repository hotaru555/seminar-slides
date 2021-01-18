---
marp: true
---

## Reverse Debugging with ETM

Wenxuan SHI, Xueying ZHANG and Haonan LI

---

# The plan

Wenxuan SHI & Xueying ZHANG: find a research topic for Group Project.

Haonan LI: NULL

---

# The work

Topic: **reverse debugging**

## Wenxuan
1. Some research on gdb reverse debugging
2. Read a paper "DoublePlay: Parallelizing Sequential Logging and Replay".

## Xueying
1. Read the document of ETMv4 to catch up with the progress.

## Haonan
Read a paper "ReVirt: Enabling Instrusion Analysis through Virtual-Machine Logging and Replay"

---

# ETM

- real time
- data trace: not supporded on ARMv8
- instruction trace:
  - PE -(some instractions)-> trace unit(resources) -> filter(programmable) -(trace stream)-> trace analyzer
  - encode(trace unit) and decode(analyzer)
  - return stack
  - synchronize information
  - contains: virtual address and 'system state' (EL, securtity state, condition, etc.)

---

# GDB: Reverse Debugging with Record and Replay

- GDB can **record** a log of process execution and save it.
- This record can be loaded later on, and used for debugging. This is called offline debugging.
- It offers the advantage that you can catch the issue once, and **replay** it as much as needed to find the root cause and fix it.

---

# Performance issue

To realize this functionality, GDB is in fact executing the software, one assembly instruction after another and **recording relevant registers and memory locations**.

This is a slow operation that can drastically change the timing of process execution, and thus **change the conditions that raise the bug.**

---

# GDB solution

- Use SoC IPs to accelerate the operation.
- GDB has support for "Processor Trace (PT)" and "Branch Trace Store (BTS)" IP on Intel processors.


## Limitation

- It doesn't support ARM
- If hardware acceleration is enabled, only **execution flow** is record. (branch record)

---

# Our work

- Use ETM to accelerate recording
- Try to trace **data flow** (knowing the exact value change in memory and register)
- Try to support debugging on **multi-core**

---

# Multicore: issue on recording

- shared memory (shared **source of truth**)
- order matters!

## Common solution

turn multicore program into a **equivalent** unicore program.

(equivalent: two program beginning at same status end at same status.)

---

# DoublePlay

K. Veeraraghavan et al., “DoublePlay: Parallelizing Sequential Logging and Replay,” ACM Trans. Comput. Syst., vol. 30, no. 1, pp. 1–24, Feb. 2012, doi: 10.1145/2110356.2110359.

---

![](doubleplay.png)

---

![](notes.png)

Details at ["My notes on DoublePlay-Parallelizing-Sequential-Logging-and-Replay"](https://www.whexy.com/2021/01/16/DoublePlay-Parallelizing-Sequential-Logging-and-Replay/)
