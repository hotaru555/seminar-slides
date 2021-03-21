---
title: "Deterministic Replay: A Survey"
author: "YUNJI CHEN, SHIJIN ZHANG, QI GUO, LING LI, RUIYANG WU, and TIANSHI CHEN,"
institute: "SKL of Computer Architecture, Institute of Computing Technology, Chinese Academy of Sciences"
urlcolor: blue
colortheme: "beaver"
date: "March 16, 2021"
theme: "Heverlee"
aspectratio: 43
lang: en-US
marp: true
---

# PRELIMINARIES

---

## Judgment Criteria

- log size
- record slowdown
- replay slowdown
- implementation cost
- probe effect


# APPLICATIONS

---

## Program Debugging

- llll

---

## Online Program Analysis

- Improve the syscall capturing, supporting record for the parameter change

---

## Postsilicon Debugging

---

## Fault Tolerance

---

## Performance Prediction

---

## Intrusion Analysis


# TAXONOMY

---

## Single-Processor/Multiprocessor

---

## Single-Threaded Program/Multithreaded Program

---

## Abstract Level

---

## MPI/Shared Memory

---

## Message Recording Style

---

## Shared-Memory Recording Style

---

## Hardware Assisted/Software Only

---

## Fidelity

# SINGLE-PROCESSOR DETERMINISTIC REPLAY SCHEMES

---

## SP Schemes Dedicated to Single-Threaded Programs

---

## SP Schemes Supporting Multithreaded Programs

# MULTIPROCESSOR DETERMINISTIC REPLAY SCHEMES

---

## Message-Passing MP Scheme

---

## Shared-Memory MP Schemes: 



---

### Complete Record Schemes

---

### Partial Record Schemes

- Only guarantee identity output.
- do not record orderings among other shared-memory operations: deduce or rely on iterative trials

Two kinds: Hardware-Assisted Schemes and Software-Only Schemes.

---

### Hardware-Assisted Schemes

- Lee et al. [2009, 2011] propose to reuse existing load-based checkpoint schemes to record only program inputs. The shared-memory dependencies can be reconstructed by offline symbolic analysis.

- By satisfying these constraints, the Satisfiability Modulo Theory(SMT)-based offline solver can find multiple valid totaling orders.

- disadvantage: analysing time increases very quickly as the number of processer increase.

---

### Hardware-Assisted Schemes

![](16.png)

---

### Software-Only Schemes: RecPlay [Ronsse and de Bosschere 1999]

A pioneer. Does not manage data race. Not practical.

---

### Software-Only Schemes: ODR

Altekar and Stoica [2009] proposed ODR (Output-Deterministic Replay). 

Mentioned lastweek.

---

### Software-Only Schemes: PRES

PRES (Probabilistic Replay with Execution Sketching) is another representative software-only partial record scheme [Park et al. 2009]. 

- PRES utilizes a depth-first heuristic algorithm to find a suspect data race that might be responsible for the failure of the last replay trial to reduce the attemps.

---

### Software-Only Schemes: PRES

![](17.png)

---

### Software-Only Schemes: Respec


To avoid trials that replay the whole execution of a program again and again, Respec [Lee et al. 2010] splits the entire execution of a program into many slices.


---

### Deterministic Parallelism Schemes.

The deterministic parallelism introduced in this part does not record any order of shared-memory operations (including synchronizations and data races), but instead forces shared-memory operations to **obey some predefined orders**.

Two kinds: Hardware-Assisted Schemes and Software-Only Schemes.

---

### Hardware-Assisted Schemes : DMP

DMP is a representative hardware-assisted deterministic parallelism scheme [Devietti et al. 2009].

- DMP-Serial a token is used to control the memory operations of all cores.  (hardware: token)
- DMP-ShTab divides each trunk into two parts: a parallel part and a serial part. (hardware: sharing table data structure)
- In the DMP-TM mode, all cores are allowed to simultaneously execute in a transactional manner. (hardware: enforce a specific transaction commit order)

disadvantage: designed for the sequential consistency model

---

### Hardware-Assisted Schemes : DMP

- DMP-TM: 

![](20.png)

---

### Hardware-Assisted Schemes : RCDC

Devietti et al. [2011] proposed RCDC to achieve deterministic parallelism on systems providing relaxed memory consistency models.

- The execution of a round of quantums has two phases: the parallel phase and the commit phase.
- Each core saves all stores performed in the parallel phase of a quantum in its local buffer (and thus they are temporarily invisible to other cores).
- In the commit phase, each quantum in the round is serially committed in a deterministic order

hardware:  precise instruction counting and store data buffering.

---

### Hardware-Assisted Schemes : RCDC

![](20.png)


### Hardware-Assisted Schemes : Calvin

Hower et al. [2011] proposed a hardware-assisted deterministic parallelism scheme named Calvin. Calvin provides **three modes**: conventional mode, bounded deterministic mode, and unbounded deterministic mode.

---

### Software-Only Schemes: Kendo

- focuse on enforcing deterministic synchronization orders.

- use deterministic logical time (DLT) to compute a deterministic yet load-balanced interleaving of synchronizations.

- disadvantage: Kendo does not consider data races

---

### Software-Only Schemes: Kendo


![](24.png)

- commits a store instruction or spins on a contested lock: logic clock +=1

---

### Software-Only Schemes: CoreDet

CoreDet [Bergan et al.2010] is a compiler and runtime system that can enforce deterministic parallelism for all shared-memory operations

- DMP-O is the software implementation of DMP-ShTab in DMP
- DMP-B executes multithread programs in rounds of quantums, each quantum in DMP-B has a fixed length. three phases: parallel phase, commit phase, and serial phase. 
- DMP-PB  partitions the whole memory into thread-private and shared spaces to reduce the amount of buffer queries.












