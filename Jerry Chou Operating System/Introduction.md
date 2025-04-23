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
-  Busy/wait is very inefficient
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

