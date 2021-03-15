---
title: "Summary for Last Week"
author: "Haonan Li, Wenxuan Shi, Xueying Zhang"
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

## Plan & Progress

- Improve the syscall capturing, supporting record for the parameter change

- Using `LD_PRELOAD` to hook library.

---

## Next week

- Improve the syscall capturing, supporting record for the parameter change

# Xueying

---

## Last Week's work

Took a TOEFL test.

---

## Next week's work

Use C++ to run a single assembly code from user input.

# Wenxuan

---

## Last week's plan

- [x] Took a TOEFL test.

- [ ] Learning ELF format.

---

## Re-Execute

### Binary
Original BIN + ETM Tracing Output + System call hooks + dynamic library hooks ==> New BIN

### Interpreter
- Dynamically analysis from Original BIN + ETM Tracing Output + System call hooks + dynamic library hooks

- Put up an instruction and change PC register every time.

---

## Benefit


### Original BIN: Loop

```assembly
<loop_begin>:
	// inside a loop
	add r0, r0, r1
	b.ne <loop_begin>
// out of loop
```

### New Binary

```assembly
add r0, r0, r1
add r0, r0, r1
add r0, r0, r1
add r0, r0, r1
... // maybe 1,000,000 times?
add r0, r0, r1
```

---

## Wrap the loop?

::: columns

:::: {.column width="20%"}

![](no_wrap.png){height=90%}

::::

:::: {.column width="80%"}

- What if we wrap the loop just like the original binary does?

- But, how to solve the Data Race problem?

- One possible solution is to wrap as much code as we can.

:::: 

:::

---

## "BIN Interpreter"

- Put an instruction to the execution queue, and move PC to the address.

- Put the instruction to the exact position in the virtual memory as it is in the original binary.


---

## One more thing

### "Hook the libC": Dynamic injection

Environment variable `LD_PRELOAD`: Linker will first load the dynamic library set in the `LD_PRELOAD` before linking all the dynamic library.


. . .

### Security Research & CTF
Dynamic injection is quite frequently used in the CTF challanges. 

`\color{gray} Please join us in the SUSTech CTF team, training carried on every Saturday.`{=latex}

---

## Next week's plan

- [ ] Use C++ to run a single assembly code from user input.

- [ ] Find an algorithm to replace branch instructions in the assembly code.
