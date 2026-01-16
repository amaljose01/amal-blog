---
layout: post
title: "Operating System Concepts: The Core of Computing"
date: 2018-06-27 10:00:00 +0000
categories: [operating-systems, systems, computer-science, tutorial]
author: Amal
image: /assets/images/posts/2018-06-15-operating-system-concepts.svg
---



# Operating System Concepts: The Core of Computing

## Introduction

An operating system manages hardware resources and provides services for applications. Understanding OS concepts is fundamental to computer science and system administration.

## Part 1: OS Fundamentals

### What is an Operating System?

An OS manages:
- **Hardware Resources**: CPU, memory, disk, peripherals
- **Processes**: Execution, scheduling
- **Memory Management**: Allocation, protection
- **File System**: Storage, access
- **Device Management**: I/O operations
- **Security**: User authentication, access control

### Types of Operating Systems

```
Batch OS         - Jobs processed in batches
Time-sharing OS  - Multiple users, time slices
Real-time OS     - Deterministic response times
Distributed OS   - Multiple computers
Embedded OS      - Devices, IoT
```

## Part 2: Process Management

### Process States

```
     ┌─────────────┐
     │   Created   │
     └──────┬──────┘
            │
            ▼
     ┌─────────────┐
     │    Ready    │─────────┐
     └──────┬──────┘         │
            │         (Preempted)
            ▼                │
     ┌─────────────┐         │
     │   Running   │─────────┘
     └──────┬──────┘
            │
      ┌─────┴──────┐
      │            │
      ▼            ▼
  Blocked      Terminated
```

### Process Scheduling

```
Scheduling algorithms:
- First Come First Served (FCFS)
- Shortest Job First (SJF)
- Priority Scheduling
- Round Robin
- Multilevel Queue Scheduling

Context Switching:
- Save process state
- Load new process state
- Incurs overhead (memory, CPU time)
```

### Inter-Process Communication (IPC)

```
Methods:
- Pipes
- Message Queues
- Shared Memory
- Semaphores
- Signals
- Sockets
```

## Part 3: Memory Management

### Memory Hierarchy

```
Register      <- Fastest, smallest
    ▲
    │
Cache L1, L2, L3
    ▲
    │
RAM (Main Memory)
    ▲
    │
SSD/HDD       <- Slowest, largest
```

### Memory Allocation

```
Contiguous Allocation:
- Fixed Partitioning
- Dynamic Partitioning
- Best Fit
- First Fit
- Worst Fit

Non-Contiguous Allocation:
- Paging
- Segmentation
```

### Virtual Memory

```
Benefits:
- Run programs larger than RAM
- Program isolation
- Easier relocation

Techniques:
- Demand Paging
- Page Replacement Algorithms
- Working Set Model
```

### Page Replacement Algorithms

```
- First In First Out (FIFO)
- Least Recently Used (LRU)
- Optimal (Belady's Algorithm)
- Second Chance Algorithm
```

## Part 4: File System

### File System Structure

```
File System
├── Boot Block
├── Super Block (metadata)
├── Inode Block
│   ├── Inode 1
│   ├── Inode 2
│   └── ...
├── Data Block
│   ├── Data 1
│   ├── Data 2
│   └── ...
└── Free Space Management
```

### File Organization

```
- Sequential Access (tape)
- Direct/Random Access (disk)
- Indexed Access (database)
```

### Directory Structures

```
Single-level: /file.txt
Two-level: /user/file.txt
Tree: /home/user/documents/file.txt
Graph (DAG): Symbolic links
```

### Common File Systems

```
FAT (File Allocation Table)
- Simple, limited file size
- Used in USB drives

NTFS (New Technology File System)
- Modern, large file support
- Windows default

ext4 (Fourth Extended)
- Linux standard
- Journaling support

APFS (Apple File System)
- macOS Mojave+
- Optimized for SSDs
```

## Part 5: Disk Management

### Disk Structure

```
Disk = Platter(s) + Head(s) + Track(s) + Sector(s)

Seek Time    - Head movement time
Rotation Latency - Platter rotation time
Transfer Time - Data reading time
Total Time = Seek + Rotation + Transfer
```

### Disk Scheduling Algorithms

```
FCFS (First Come First Served)
- Simple, fair, but slow

SSTF (Shortest Seek Time First)
- Fast, but can starve tracks

SCAN (Elevator Algorithm)
- Moves in one direction
- Good average performance

C-SCAN (Circular SCAN)
- Always scans in one direction
- More uniform wait time

LOOK and C-LOOK
- Stop at last request, don't go to end
```

## Part 6: Deadlocks

### Deadlock Conditions

All four must be present:

```
1. Mutual Exclusion - Resource can't be shared
2. Hold and Wait - Process holds resource while waiting
3. No Preemption - Can't forcibly take resource
4. Circular Wait - Circular chain of processes
```

### Deadlock Prevention

```
Prevent one of the four conditions:
- Mutual Exclusion - Can't prevent
- Hold and Wait - Request all resources upfront
- No Preemption - Allow preemption
- Circular Wait - Resource ordering
```

### Deadlock Avoidance

```
- Banker's Algorithm
- Safe State Analysis
- Resource Allocation Graph
```

## Part 7: Security and Access Control

### Access Control Models

```
Discretionary Access Control (DAC)
- User-based permissions
- Linux/Unix model

Mandatory Access Control (MAC)
- Policy-based
- SELinux model

Role-Based Access Control (RBAC)
- Role-based permissions
```

### File Permissions (Unix/Linux)

```
rwx rwx rwx
└┬┘ └┬┘ └┬┘
 │   │   └─ Others
 │   └───── Group
 └────────── Owner

r (read)    = 4
w (write)   = 2
x (execute) = 1

644 = rw- r-- r--
755 = rwx r-x r-x
```

## Part 8: Virtualization

### Types of Virtualization

```
Full Virtualization - Complete simulation of hardware
Para-virtualization - Partial simulation, efficient
Container - OS-level virtualization, lightweight
```

### Hypervisors

```
Type 1 (Bare Metal)
- ESXi, Hyper-V
- Direct hardware access
- High performance

Type 2 (Hosted)
- VirtualBox, VMware Player
- Runs on host OS
- Easier to use
```

## Summary

Key OS Concepts:
- **Process Management**: Scheduling, context switching
- **Memory Management**: Virtual memory, paging
- **File System**: Organization, access methods
- **Disk Management**: Scheduling, I/O optimization
- **Deadlocks**: Prevention and avoidance
- **Security**: Access control models
- **Virtualization**: Efficient resource utilization

Mastering these concepts prepares you for advanced system administration and development!
