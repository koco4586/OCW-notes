# Computer system
-  User: people, machines, other computers.
-  Application: define the ways in which the system resources are used to solve the computing problems.
-  Operating System (OS): controls and coordinates the use of the hardware/resources.
-  Hardware: provides basic computing resources.
# What is an OS?
- An OS is the permanent software that controls/abstracts hardware resources for user applications.
- Multi-tasking OS:
    - Manages resources and processes to support different user applications.
    - Provides Applications Programming Interface (API) for user applications.
- ![image](https://github.com/user-attachments/assets/1ec1d32d-b2b8-4bb0-83f9-de94d4b55e42)
- Definition of OS:
    - **Resource allocator**: *manages* and *allocates* resources to insure efficiency and fairness.
    - **Control program**: *controls* the execution of *user programs* and operations of *I/O devices* to prevent errors and improper use to computer.
    - **Kernel**: the one programs running at all times.
- Goals of an OS:
    -  Convenience.
    -  Efficiency.
- Importance of an OS:
    -  System API are the *only* interface between user applications and hardware.
    -  OS code cannot allow any bug.
    -  OS and computer architecture influence each other.
# Interrupt I/O
-  Busy/wait is very inefficient![image](https://github.com/user-attachments/assets/5a9e1955-e4cf-436f-bc43-67eca63370b1)

    - CPU cannot do other work while testing device.
    - Hard to do simultaneous I/O.
-  **Interrupts** allow a device to change the flow of control in the CPU.
-  The interrupt timeline for a single process![image](https://github.com/user-attachments/assets/b3f08eca-6470-4837-88a6-fc81085964ee)
- Interrupt-Driven I/O![image](https://github.com/user-attachments/assets/853f1f10-9a57-4972-a339-7b7dd5b66c53)
- Modern OS are *interrupt driven*.
    - *Hardware* may trigger an interrupt at any time by sending a *signal* to CPU. (Interrupt vectors are determined by the hardware and have a fixed number of signals.)![image](https://github.com/user-attachments/assets/951acd4b-c7c1-42b0-98af-592eaa7f8a33)
    - *Software* may trigger an interrupt either by an *error* or by a user request for an *OS service* (system call), software interrupt also called *trap*. (switch-case statements are commonly used to handle different types of system calls based on an interrupt number)![image](https://github.com/user-attachments/assets/dcb8008c-b705-4595-846a-b722193221f1)
- Common Functions of Interrupts:
    - Interrupt transfers control to the *interrupt service routine* (ISR) generally, through the interrupt vector (or switch case), which contains the addresses (function pointer) of all the service routines.
    - Interrupt architecture must save the *address* of the interrupted instruction.
    - Incoming interrupts are *disabled* while another interrupt is being processed to prevent a *lost interrupt*.
# Storage hierarchy
- ![image](https://github.com/user-attachments/assets/e7ae2a4e-73da-47c2-98d5-afd3c5d9c561)

- ![image](https://github.com/user-attachments/assets/dc67725f-e99c-4022-a934-29397c531581)
# Cache coherence 
- The following are the requirements for cache coherence:
    -  Write Propagation:
Changes to the data in any cache must be propagated to other copies (of that cache line) in the peer caches.
    - Transaction Serialization:
Reads/Writes to a single memory location must be seen by all processors in the same order.
- ![image](https://github.com/user-attachments/assets/47857840-be4b-4de5-9f85-5405d7f5210a)
- MESI protocol:

| State | Full Name  | Meaning   | Description                                                                |
|-------|------------|-----------|----------------------------------------------------------------------------|
| M     | Modified   | Changed   | The cache line is modified and only exists in this cache. Memory is stale. |
| E     | Exclusive  | Owned     | The cache has the only copy. It matches main memory.                       |
| S     | Shared     | Unchanged | Multiple caches may have the same, unmodified copy.                       |
| I     | Invalid    | Not usable| The cache line is invalid and must be reloaded from memory or another cache.|
- The State Transitions: ![image](https://github.com/user-attachments/assets/8dbe14c5-dee4-41ce-bbac-1dc8f64da4d0)
# Hardware protection
- Dual-Mode Operation
    - Provide hardware support to differentiate between two modes of operations:
        - **User mode**: execution done on behalf of a user.
        - **Monitor mode (kernel mode)**: execution done on behalf of OS.
    - Mode bit added to computer hardware to indicate the current mode: kernel (0) or user (1).
    - When an interrupt/trap or fault occurs, hardware switches to monitor mode.
    - **Privileged instructions**:
        - Executed only in monitor mode.
        - Requested by users (system calls). 
- I/O Protection
    - ALL I/O instuctions are privileged instructions.
    - Must ensure that a user program could never gain control of the computer in monitor mode.  
- Memory Protection
    - Protect:
        - Interrupt vector and the ISR.
        - Data access and over-write from other programs.
    - HW support two registers for legal address determination:
        - **Base register**: holds the smallest legal physical memory address.
        - **Limit register**: contains the size of the range.
    - Memory outside the defined range is protected.
    - Hardware address protection: ![image](https://github.com/user-attachments/assets/0881cebb-94c8-400b-ba5c-4fe4f747f210)
 
- CPU Protection
    - Prevent user program from not returning control.
        - getting stuck in an infinite loop.
        - not calling system services.
    - HW support: **Timer** interrupts computer after specified period
        - Timer is decremented every clock tick.
        - When timer reaches value 0, an interrupt occurs.   
    - Timer commonly used to implement time sharing
    - **Load-timer(overwrite)** is a privileged instruction.





