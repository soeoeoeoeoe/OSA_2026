# Lab 6 — Live processes and memory pressure

Pair: Inzhu + Diana · Date: 18.11 - 19.11 · Guests: Ubuntu/ Windows

## Part 1 — One process, three tools

Process chosen: VBoxService — why: It is the VirtualBox Guest Additions background service present on both guest operating systems, making it ideal for comparing how Windows and Linux report process metadata and memory.

| --- | Task Manager | Process Explorer |	htop / ps |
| --- | --- | --- |
|PID |	1952 |	1952 |	1038 |
| Parent (name and PID) |	— (not shown) |	services.exe (920) |	systemd (1) |
| "Memory" as shown by default | 1,120 K (Memory private working set) |	2,464 K (Private Bytes) |	0.3% (MEM%) / 3,832 K (RES) |
| Private memory |	1,120 K (Memory private) / 2,412 K (Commit size) |	2,464 K (Private Bytes) |	— (no single column; ~412 K non-shared RSS) |
| Working set / RES |	9,940 K (Working set) |	9,644 K (Working Set) |	3,832 K (RES / RSS) |
| Virtual size |	2,412 K (Commit size) |	2,464 K (Private Bytes) |	285M (VIRT) / 292,100 K (VSZ) |
| Handles / file descriptors |	200 |	200 |	5 (open FDs in /proc/1038/fd) |
| Threads |	14 |	14 |	9 (nlwp) |

### Where the tools disagree
<at least one disagreement, with the reason>

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
