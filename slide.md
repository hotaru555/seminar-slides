---
title: "Summary for Last Week"
author: "Haonan **Lu**, Wenxuan Shi, Xueying Zhang"
institute: "COMPASS"
urlcolor: blue
colortheme: "beaver"
date: "March 9, 2021"
theme: "Heverlee"
aspectratio: 43
lang: en-US
marp: true
---
# Haonan

## Plan of Last Week

- Capture syscalls
	- *Deploy sysdig*
	- Write a kernel module manually to replace sysdig
- Make more experiments
	- Figure out the overhead 
	- Judge whether the information captured is enough 
	- Make a demo to finish original schedule 

---


## Actual Progress

- Capturing syscall: [Github](https://github.com/Tert-butyllithium/Mysisdig)
	- Finish a basic version (*print log*, *condition filter*)
	- Investigate different types (i.e., passing different parameters) of syscalls 
	- Set this project as my *graduation project*


---

## Demo: Capturing Syscall {.allowframebreaks}

For example,  we have a syscall `getrandom`:

```c
ssize_t getrandom (void *__buffer, size_t __length,
				   unsigned int __flags)
```

And there is the schema of syscall in Arm64:


| **arch**  | syscall NR | **return** | **arg0** | arg1 | arg2 | arg3 | arg4 | arg5 |
| :---: | :--------: | :----: | :--: | :--: | :--: | :--: | :--: | :--: |
| arm64 |     x8     |   x0   |  x0  |  x1  |  x2  |  x3  |  x4  |  x5  |
Table: Syscall Calling Convention for Arm64

Then we call it in our code:
```c
int main(){
    int a;
    getrandom(&a,sizeof a,GRND_RANDOM);
}
```

The capturing snippet show as follow: *but `regs[0]` was replaced by `ret`*

```bash
[my_sysdig:] syscall 0x116, with pid=0x178e, name=a.out
[my_sysdig:] exit syscall, regs[0]=0x4, with pid=0x178e
```

There are 26 syscalls (arm64) similar to `getrandom`, and we need to save their `regs[0]` when entering the syscall. 

---

## How to Save Parameters

- Set a global variable at `entering` and get its value at `exit`? 
	- No, multiple system calls can exist *at the same time* 
	- Considering multithread: When the program is waiting for a syscall (such as `read`), other threads can still run normally or call syscalls.
- Maintain a data struct to save `regs[0]` 
	- Syscall will block the thread, so we could save a value for each thread number.

---

## Plan

- Improve the syscall capturing, support record for the parameter change
- Further discuss with Yiming how to handle library calls
- Read a paper for sharing next week...

# Xueying

## Last Week's work

Read a paper " iReplayer: In-situ and Identical Record-and-Replay for Multithread Applications".

---

## Features

- in-situ replay: in the same process
- identical replay: strictly preserves all system states, results of system calls, the order and results of synchronizations, and the same memory allocations/deallocations
- high efficiency: 3% recording overhead

---

## Overview

- Divide the entire execution into several epochs, based on irrevocable system calls, abnormal exits and user-defined criteria, and always replay the last-epoch. 
- When a thread encounters the end of one epoch, it will coordinate with other threads to pause the execution. And then choose to roll-back or not. 
- Record: the order of synchronization and system calls in the same thread and across multiple threads with a special data structure

---

## Identical replay

- Race of memory access: try many times
- System call: make delicate classification. Repeatable, recordable, revocable(files), deferrable(delay) and irrevocable. 
- Per-thread heap and a super heap (from other articles). 

---

## conclusion

Not very useful for us. The key point of this article concentrates on how to record and how to synchronize multi-threading, which we don’t need to take into account.

---

## Next week’s plan

Take a TOFEL test

# Wenxuan

---

## Record & Replay: What have we done?

### Record

Combine binary and ETM Tracing output (`br` instructions, `ldr` instructions, syscall)

### Replay (offline)

Reproduction of concurrency bug.

---

## How to reproduce concurrency bug?

### Execution Order

Concurrency is caused by unexcepted instruction execution order.

During reproduction, ensure the execution order is the same.

### Solution

Add delay to make sure every thread visit shared memory in the correct order. (Hardware Breakpoint)

---

## Challanges: Towards the real replay machine

### e.g. A bug caused by system call

```rust
fn bug_caused_by_syscall() {
    let magic_pid = getpid();
    if magic_num == 114514 {
        panic!();
    } else {
        // behave_normally
    }
}
```

###

System call hook *(my_sysdig, Haonan)* is needed for recovering data flow. It happened that it can be used for replay, too.

---

## Is that all?

Due to *DoublePlay: Parallelizing Sequential Logging and Replay*, we should ensure these three aspects are exactly the same.

- [x] The order and results of system calls.
- [ ] Signals
- [ ] Low-level synchronization operations in GNU libc.

---

## More circumstances

### Signals

Received a signal and jump to execute the programmer-defined handler.

```c++
int main() {
    // do something
    
    // Jump to handle a signal.
    // System status changed.

    // continue
}
```

It is the other way to switch to kernel: Operating System traps it, without an explicit **system call** invoking. 

### Synchronization

Find a way to add delay directly to the binary file?

---

## Goal

- [ ] Turn ETM tracing output into an executable binary file (native code).

- [ ] Find `svc` instructions (system call) and feed it with given values (system call hook).

- [ ] Implement a tool to control the binary file, or use gdb.

---

## Learning ELF files

This week I'm learning ELF format.

---

## Next week’s plan

- Try to turn control flow into an executable binary file (without modification)

- Take a TOFEL test