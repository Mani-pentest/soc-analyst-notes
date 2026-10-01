# WIN05: Windows Internals — Processes, Threads, Memory, PE Format

A process maintains and represents the execution of a program; an application can contain one or more processes. Processes have several components and directly interact with memory, making them a key target for both defenders and attackers to understand.

## Process Components

| Component | Purpose |
|---|---|
| Private Virtual Address Space | Virtual memory addresses the process is allocated |
| Executable Program | Defines code and data stored in the virtual address space |
| Open Handles | Handles to system resources accessible to the process |
| Security Context | Access token defining user, security groups, privileges, and other security information |
| Process ID (PID) | Unique numerical identifier of the process |
| Threads | Section of a process scheduled for execution |

## Lower-Level Process Components (In Memory)

| Component | Purpose |
|---|---|
| Code | Code to be executed by the process |
| Global Variables | Stored variables |
| Process Heap | Defines where data is stored |
| Process Resources | Further resources of the process |
| Environment Block | Data structure defining process information |

## Threads

A thread is the unit of execution scheduled within a process — effectively the "worker" inside the process container.

**Note:** Threads share the process's memory space, but each thread maintains its own stack.

## Virtual Memory

Virtual memory allows internal components to interact with memory as if it were physical memory, without risk of collisions between applications. It provides each process with a private virtual address space.

**Note:** A 32-bit x86 system has a theoretical maximum virtual address space of 4GB.

## DLL (Dynamic Link Library)

A library that contains code and data that can be used by multiple programs simultaneously — reduces redundancy across applications sharing common functionality.

## PE (Portable Executable) Format

The PE format defines information about an executable and its stored data, along with the structure of how those data components are organized. It's made up of PE and COFF (Common Object File Format) files.

**Note:** The DOS Stub component of a PE file is responsible for printing "This program cannot be run in DOS mode" when the executable is run in an incompatible environment.

## Windows API and Kernel Interaction

The Windows API provides native functionality to interact with the Windows OS, made up of the Win32 API and Win64 API. By default, an application cannot directly interact with the Windows kernel or modify physical hardware — it requires this API interface. The switch between user mode and kernel mode is facilitated by system and API calls.

**Note:** Understanding normal process/thread behavior is foundational to both defensive and offensive security work — attackers frequently abuse legitimate processes (e.g., LSASS) to hide malicious activity, and this same internals knowledge underpins techniques like DLL injection and process exploitation covered later in the pentest track.

## Why It Matters

Attackers abuse legitimate processes (e.g., LSASS) to hide malicious activity, making them harder to spot without a baseline of what normal process behavior looks like. This chapter's internals knowledge underpins both directions of security work:

- **SOC/Defensive:** Spotting malicious behavior requires first understanding what normal process, thread, and memory behavior looks like
- **Pentest/Offensive:** DLL injection and other memory-based exploitation techniques directly depend on understanding processes, threads, and virtual memory covered here

## Summary

- A process is a container that represents a running program; a program can spawn multiple processes
- Threads are the workers inside a process — they share process memory but each keeps its own stack
- Virtual memory gives each process an isolated, private address space, invisibly mapped to physical RAM
- DLLs are shared code libraries used by multiple programs at once — and the underlying mechanism behind DLL injection
- The PE format defines how `.exe`/`.dll` files are structured, built from PE + COFF, and carries a legacy DOS stub
- The Windows API (Win32/Win64) is the only sanctioned path from user mode into kernel mode, via system/API calls
- This chapter is conceptual groundwork — later Windows exploitation and SOC detection topics both build on it
