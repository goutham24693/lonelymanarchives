---
layout: post-wrapper
title:  "User programs vs Kernel Modules"
date:   2019-05-08 22:00:40 +0530
categories: jekyll update
---

<h3>What is Processor Privilege rings?</h3>
<p>
Before moving into difference between user and kernel programs, it is better to understand CPU Ring levels and why they are used. The Operating System accounts for independent operation of programs and protection against unauthorized access to resources. To ensure this Processor enforces protection of system software from the user programs using "Run levels". 
</p>

<p>
Processors like x86 family have atleast 2 run levels. The kernel runs in the highest level -supervisor mode and the other user applications run in lowest level called user modes. This segregation of run level is termed as "Privilege Rings".
</p>

<h3>User Mode vs Prilileged mode:</h3>
<p>
In User mode, the processor restricts direct access to hardware and unauthorized access to memory. A User program may access these restricted areas by means of System Call. Once the user program calls a system call, the kernel transfers execution from user space (user mode) to kernel space (privileged mode) and does the restricted operation on behalf of the user program.
</p>
<p>
The Kernel and the dynamically loadable kernel modules work in privileged mode. They are responsible for safe operation of the computer system and hardware devices and this is the reason why they run in the highest level of privilege. 
</p>

<h3>User space vs Kernel Space:</h3>
<p>
User application has a very large stack area but the kernel has smaller stack. It is not recommended declare a large automatic variables or larger structure inside kernel modules. Dynamic allocation should be prefered in kernel space, else stack size will grow and the useable memory will be reduced. Unlike user applications, Kernel code can not handle floating points. Standard C library such as glibc function calls can not be used in kernel space. Kernel code has a seperate set of symbols and variables. Only functions that are actually part of kernel itself may be use these symbols and function calls.
</p>
<p>
Segmentation faults in a user program causes paticular program to crash, but an illegal memory access in kernel space can result in corrupting the boot partition or can cause a kernel panic. Kernel handles the user program operations, so it can trap unauthorized memory access, but this control check can not be done inside kernel itself. A Kernel panic will cause the computer to crash and will require to restart the machine.
</p>

