xv6 RISC-V Kernel Enhancement Project

Ten new features for the xv6 operating system: five from Operating Systems, five from Computer Architecture.

⸻

Project Information

* Student Name: MISBAH UL HAQ
* Roll No.: BCS-F24-E34
* Course: OPERATING SYSTEM AND COMPUTER ARCHITECTURE
* Instructor: SIR RAZA
* Base OS: xv6-riscv (MIT)

⸻

Introduction

xv6 is a small teaching operating system based on Unix Version 6, written for the RISC-V processor. This project expands its kernel with ten new modules. The first five show how an operating system manages processes, memory and files. The last five show how the kernel works directly with RISC-V hardware.

Quick Start

git clone <repository-url>
cd xv6-riscv
make qemu

Inside xv6, type the module’s command name (listed under each module) to run it. Press Ctrl+A then X to leave QEMU.

Repository Structure

xv6-riscv/
├── kernel/        # modified kernel files
├── user/          # new user programs (tests for each module)
├── Makefile       # updated with new programs
├── screenshots/   # output of every module
└── README.md

⸻

Part I: Operating System Modules

Module 1: Child Process Counter (getchildcount)

Objective: Find out how many child processes the calling process currently has.

Implementation: sys_getchildcount() loops through the process table and counts entries whose parent field points to the calling process (hold the proper lock while checking).

Files: kernel/sysproc.c, kernel/syscall.h, kernel/syscall.c, user/user.h, user/usys.pl, user/childtest.c

Run: $ childtest (forks a few children, then prints the count)

What you learn: Parent-child relationships and the process table.

⸻

Module 2: clear Screen Utility

Objective: Clear the terminal screen inside xv6.

Implementation: A user program that prints the ANSI escape sequence for “clear screen and move cursor to top-left” using write() to the console.

Files: user/clear.c, Makefile

Run: $ clear

What you learn: Console output and how a shell command is built and added to UPROGS.

⸻

Module 3: Process Memory Size (memusage)

Objective: Report how much memory the current process is using.

Implementation: sys_memusage() returns myproc()->sz, the size of the process address space in bytes. The test program allocates memory with sbrk() and shows the value before and after.

Files: kernel/sysproc.c, kernel/syscall.h, kernel/syscall.c, user/user.h, user/usys.pl, user/memtest.c

Run: $ memtest

What you learn: Process address space and heap growth.

⸻

Module 4: head Utility

Objective: Display only the first N lines of a file.

Implementation: A user program that opens the file, reads it in small blocks with read(), counts newline characters and stops after N lines (default 10).

Files: user/head.c, Makefile

Run:

$ head README
$ head -5 README

What you learn: File system calls: open, read, write, close.

⸻

Module 5: Process State Checker (getprocstate)

Objective: Return the current state of any process, given its PID.

Implementation: sys_getprocstate(pid) searches the process table for the PID and returns its state (SLEEPING, RUNNABLE, RUNNING, ZOMBIE) as a number, or -1 if not found.

Files: kernel/sysproc.c, kernel/syscall.h, kernel/syscall.c, user/user.h, user/usys.pl, user/statetest.c

Run: $ statetest

What you learn: The process life cycle and states.

⸻

Part II: Computer Architecture Modules

Module 6: Register Viewer (sp, ra, tp)

Objective: Display the values of important CPU registers.

Implementation: Helper functions r_sp(), r_ra() and r_tp() in kernel/riscv.h read the stack pointer, return address and thread pointer with inline assembly. A syscall prints them.

Files: kernel/riscv.h, kernel/sysproc.c, kernel/syscall.h, kernel/syscall.c, user/user.h, user/usys.pl, user/regview.c

Run: $ regview

What you learn: The RISC-V register set and calling convention.

⸻

Module 7: SATP Register Reader

Objective: Show the address translation setup through the satp register.

Implementation: r_satp() reads the register. The syscall splits the value into the mode field (8 means Sv39) and the physical page number of the root page table, then prints both.

Files: kernel/riscv.h, kernel/sysproc.c, kernel/syscall.h, kernel/syscall.c, user/user.h, user/usys.pl, user/satptest.c

Run: $ satptest

What you learn: How the hardware finds the page table.

⸻

Module 8: Interrupt Status Checker

Objective: Show whether interrupts are enabled on the CPU.

Implementation: The syscall checks the SIE bit of the sstatus register using intr_get() and prints “enabled” or “disabled”. A second call demonstrates intr_off() and intr_on().

Files: kernel/sysproc.c, kernel/syscall.h, kernel/syscall.c, user/user.h, user/usys.pl, user/intrtest.c

Run: $ intrtest

What you learn: Interrupt enable bits and why kernels disable interrupts in critical sections.

⸻

Module 9: Virtual to Physical Address Translator

Objective: Convert a virtual address of the current process into its physical address.

Implementation: sys_v2p(va) calls walkaddr() on the process page table and returns the physical address, or 0 if the page is not mapped. The test program passes the address of one of its own variables.

Files: kernel/vm.c (if helper needed), kernel/sysproc.c, kernel/syscall.h, kernel/syscall.c, user/user.h, user/usys.pl, user/v2ptest.c

Run: $ v2ptest

What you learn: Address translation using page tables.

⸻

Module 10: Memory Access Pattern Benchmark

Objective: Show that the order of memory access affects speed.

Implementation: A user program fills a small array, then reads it sequentially and with a large stride. Time for each pass is measured in uptime() ticks, and the results are compared.

Files: user/membench.c, Makefile

Run: $ membench

What you learn: Memory hierarchy, locality of reference and caching.

⸻

Summary

No.	Part	Module	Kind
1	OS	getchildcount	System call
2	OS	clear	User program
3	OS	memusage	System call
4	OS	head	User program
5	OS	getprocstate	System call
6	CA	Register viewer	System call
7	CA	SATP reader	System call
8	CA	Interrupt status	System call
9	CA	v2p translator	System call
10	CA	Memory benchmark	User program

Screenshots

Output screenshots for all ten modules are stored in the screenshots/ folder, named module1.png to module10.png.

References

* xv6-riscv source: https://github.com/mit-pdos/xv6-riscv
* MIT 6.1810 course page: https://pdos.csail.mit.edu/6.1810/
* xv6: a simple, Unix-like teaching operating system (xv6 book)

This repository is part of academic coursework. The original xv6 code belongs to its respective authors.

⸻

Mermaid Diagrams

Overall Project Architecture

flowchart TD
    A[xv6 RISC-V Operating System]
    A --> B[Operating System Modules]
    A --> C[Computer Architecture Modules]
    B --> B1[Module 1: Child Process Counter]
    B --> B2[Module 2: Clear Utility]
    B --> B3[Module 3: Process Memory Size]
    B --> B4[Module 4: Head Utility]
    B --> B5[Module 5: Process State Checker]
    C --> C1[Module 6: Register Viewer]
    C --> C2[Module 7: SATP Register Reader]
    C --> C3[Module 8: Interrupt Status Checker]
    C --> C4[Module 9: Virtual to Physical Address]
    C --> C5[Module 10: Memory Access Benchmark]
    B1 --> D[Kernel]
    B3 --> D
    B5 --> D
    C1 --> D
    C2 --> D
    C3 --> D
    C4 --> D
    B2 --> E[User Programs]
    B4 --> E
    C5 --> E
    D --> F[RISC-V Hardware]
    E --> D

System Call and Execution Flow

flowchart LR
    A[User runs command] --> B[User Program]
    B --> C{System Call Required?}
    C -->|Yes| D[System Call Interface]
    D --> E[Kernel]
    E --> F[RISC-V Hardware]
    F --> E
    E --> D
    D --> B
    B --> G[Output]
    C -->|No| B

Project Module Classification

flowchart TD
    A[Ten xv6 Enhancements]
    A --> B[Operating Systems]
    A --> C[Computer Architecture]
    B --> B1[getchildcount]
    B --> B2[clear]
    B --> B3[memusage]
    B --> B4[head]
    B --> B5[getprocstate]
    C --> C1[Register Viewer]
    C --> C2[SATP Reader]
    C --> C3[Interrupt Checker]
    C --> C4[Virtual to Physical Translation]
    C --> C5[Memory Benchmark]
