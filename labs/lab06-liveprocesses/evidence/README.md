# Lab 6 — Live processes and memory pressure

Pair: Inzhu + Diana · Date: 18.11 - 19.11 · Guests: Ubuntu/ Windows

## Part 1 — One process, three tools

Process chosen: VBoxService — why: It is the VirtualBox Guest Additions background service present on both guest operating systems, making it ideal for comparing how Windows and Linux report process metadata and memory.

| --- | Task Manager | Process Explorer |	htop / ps |
| --- | --- | --- | --- |
|PID |	1952 |	1952 |	1038 |
| Parent (name and PID) |	— (not shown) |	services.exe (920) |	systemd (1) |
| "Memory" as shown by default | 1,120 K (Memory private working set) |	2,464 K (Private Bytes) |	0.3% (MEM%) / 3,832 K (RES) |
| Private memory |	1,120 K (Memory private) / 2,412 K (Commit size) |	2,464 K (Private Bytes) |	— (no single column; ~412 K non-shared RSS) |
| Working set / RES |	9,940 K (Working set) |	9,644 K (Working Set) |	3,832 K (RES / RSS) |
| Virtual size |	2,412 K (Commit size) |	2,464 K (Private Bytes) |	285M (VIRT) / 292,100 K (VSZ) |
| Handles / file descriptors |	200 |	200 |	5 (open FDs in /proc/1038/fd) |
| Threads |	14 |	14 |	9 (nlwp) |

### Where the tools disagree
1.Memory column and Working Set / RES: Task Manager's default "memory" column shows the private working set (1,120 K), whereas Process Explorer's default column shows Private Bytes (2,464 K), and htop shows total physical RES (3,832 K). They display different numbers because they measure different types of memory allocation by default.
2.Commit size, Private Bytes and VIRT: Task Manager's Commit size (2,412 K) and Process Explorer's Private Bytes (2,464 K) measure committed virtual memory and nearly match, whereas htop has no equivalent commit column and instead shows VIRT (285M), which represents the entire virtual address space reserved by the process rather than actual RAM in use.
3.Parent process visibility: Task Manager's Details tab hides the parent process by default (—), while Process Explorer displays the process tree under services.exe (920) and htop tree view (F5) shows systemd (1).
Handles and File Descriptors: Windows reports 200 handles in both Task Manager and Process Explorer because Windows counts kernel objects like events, registry keys, mutexes, and threads as handles, whereas Linux separates threads (9) from open file descriptors (5).
## Part 2 — Memory pressure
Guest RAM: <…> · swap: <…>
Steps: <the commands, in order>
| | Before | During (worst) | After |
| Available | | | |
| Swap used | | | |
What happened: <responsiveness, what happened to the process, with times>

### What the kernel did
<the OOM line(s), quoted, and your explanation: which process, why, when>

### Windows
<the Get-WinEvent command and its result, and your explanation>

## What did not work the first time
<at least one real thing>
