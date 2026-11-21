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
1.Memory Column and Working Set / RES: Task Manager’s default "Memory" column tracks the private working set, whereas Process Explorer defaults to Private Bytes, and htop reports total physical Resident Set Size (RES). Because each tool measures a different layer of memory allocation by default, they display different values for similar processes.

2.Commit Size, Private Bytes, and VIRT: Task Manager's Commit Size and Process Explorer's Private Bytes both measure committed virtual memory, resulting in nearly identical figures. In contrast, htop lacks a direct commit-size equivalent and instead displays VIRT, which reflects the entire virtual address space reserved by a process rather than actual physical RAM in use.

3.Parent Process Visibility: Task Manager’s Details tab omits parent-child relationships by default, whereas Process Explorer clearly maps out the hierarchy in a process tree (such as nesting services under services.exe), and htop's tree view (F5) traces processes back to systemd.

4.Handles and File Descriptors: Windows reports identical handle counts across both Task Manager and Process Explorer because the OS groups various kernel objects—including events, registry keys, mutexes, and threads—under a single unified handle metric. Linux takes a more granular approach, tracking threads separately from open file descriptors (which are limited to files, sockets, and pipes, and viewed outside htop via tools like lsof or /proc).
## Part 2 — Memory pressure
Guest RAM: 1.4 GiB (1.4 Gi total in free -h) · swap: 0 B (disabled / no swap configured) Steps:
```
powershell
free -h
swapon --show
{ echo "=== BEFORE $(date) ==="; free -h; } > memory-pressure.txt
while true; do echo "=== $(date +%T) ==="; free -h; sleep 3; done | tee -a memory-pressure.txt
stress-ng --vm 2 --vm-bytes 70% --vm-method write64 --timeout 60s
stress-ng --vm 2 --vm-bytes 90% --vm-method write64 --timeout 60s
stress-ng --vm 2 --vm-bytes 120% --vm-method write64 --timeout 90s
{ echo "=== AFTER $(date) ==="; free -h; } >> memory-pressure.txt
journalctl -k --since "30 min ago" | grep -i -E -B3 -A8 "out of memory|oom-kill" > kernel-log-extract.txt
```
What Happened: With swap disabled (0 B), attempting to allocate 120% of memory beginning at 18:14:48 quickly consumed the available 1.1 GiB of RAM, depleting it to just 18 MiB by 18:15:03. Before the Out-of-Memory (OOM) killer stepped in, the system experienced intense memory thrashing and CPU starvation—keyboard input became completely unresponsive, systemd recorded watchdog timeouts past the 3-minute threshold, and the kernel logged a soft lockup with CPU#1 stalled for over 300 seconds. At 18:15:04, the Linux kernel OOM killer terminated the stress-ng-vm worker (PID 6246) along with its sibling process (PID 6250), instantly reclaiming approximately 1.26 GB of anonymous RSS memory and returning system responsiveness and available RAM (1.1 GiB) to normal by 18:15:06.

### What the kernel did
```
powershell
Nov 18 18:15:04 localhost kernel: Out of memory: Killed process 6246 (stress-ng-vm) total-vm:1478348kB, anon-rss:1264252kB, file-rss:504kB, shmem-rss:16kB, UID:1000 pgtables:2488kB oom_score_adj:1000 Nov 18 18:15:04 localhost kernel: oom_reaper: reaped process 6246 (stress-ng-vm), now anon-rss:0kB, file-rss:0kB, shmem-rss:16kB
```
Terminated Process: The stress-ng-vm worker (PID 6246), followed immediately by its sibling worker (PID 6250).

Reason for Selection: The process held the largest memory footprint (anon-rss of 1,264,252 kB, or roughly 1.26 GB out of the 1.4 GB total guest RAM) alongside the maximum oom_score_adj value of 1000. Terminating it eliminated the primary driver of the memory exhaustion and provided the quickest path to reclaiming system RAM.

Timestamp: 18:15:04 on November 18, aligning precisely with the peak memory depletion point of the 120% stress-ng workload.
### Windows
Command:
```
powershell
Get-WinEvent -FilterHashtable @{LogName='System'; ProviderName='Microsoft-Windows-Resource-Exhaustion-Detector'} -MaxEvents 5 | Format-List TimeCreated, Id, Message
```
Result:
```
powershell
Get-WinEvent : No events were found that match the specified selection criteria.
```
Explanation: Because the Windows guest never depleted its virtual memory (commit charge), no Resource-Exhaustion-Detector events were generated. Unlike Linux, Windows lacks an aggressive Out-of-Memory (OOM) killer to automatically terminate processes. Instead, when virtual memory approaches exhaustion, Windows records Event ID 2004 (identifying the top three memory-consuming processes), alerts the user, and begins rejecting new memory allocation requests.

## What did not work the first time
**Default `stress-ng` Failed with `SIGILL`:** Running `stress-ng --vm 2 --vm-bytes 70%` without explicitly defining a VM method caused the worker processes to crash immediately (`caught SIGILL, exit status=2 in 0 secs`). This occurred because VirtualBox did not expose specific CPU instructions required by one of `stress-ng`'s default memory stressors. Specifying `--vm-method write64` bypassed those unsupported instructions and resolved the crash.

**Empty Handle List in Process Explorer:** Inspecting `VBoxService.exe` properties in Process Explorer without administrator privileges displayed `<access denied>` in the User field and prevented individual handle names from appearing in the lower pane, even though Task Manager still displayed the aggregate handle count (`200`).

## Evidence
[evidence/windows lab6 1st photo](evidence/windows lab6 1st photo)- Windows Task Manager details for VBoxService.exe.

