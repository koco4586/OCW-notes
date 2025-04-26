# Process concept
- Program: binary **stored in disk** (passive entity).
- Process: a **program in execution in memory** (active entity).
- A process includes:
  - **Code** section (text section).
  - **Data** section: global variables.
  - **Stack**: **temporary local** variables and functions.
  - **Heap**: **dynamic allocated** variables or classes.
  - Current activity (**program counter**, register contents).
  - A set of associated **resources** (e.g. open file handlers).
- Process in memory: ![image](https://github.com/user-attachments/assets/f098d1c5-3d9a-4f02-aa34-d7645a68e0de)
- Threads:
  - Lightweight process: basic unit of CPU utilization.
  - All threads **belonging to the same process**, sharing:
    - **Code** section, **Data** section, OS resources.
  - ![image](https://github.com/user-attachments/assets/1baed76a-fc4b-4173-99ef-6541c4406ab4)
- Process state:
  - States:
    - **NEW**: the process is being created.
    - **Ready**: the process is in the memory waiting to be assigned to a processor.
    - **Running**: instructions are being executed by CPU.
    - **Waiting**: the process is waiting for events to occur.
    - **Terminated**: the process has finished execution.
  - Diagram of process state: ![image](https://github.com/user-attachments/assets/ee5ae9d2-7131-4aa2-bca1-8450a7697ad1)
  - Only one process is running on any processor at any instant.
  - Many processes may be ready or waiting (in a waiting queue).
- Process Control Block (PCB)
  - A Process Control Block (PCB) is a data structure used by the OS to manage information about a process
  - ![image](https://github.com/user-attachments/assets/923fe31e-a148-4bfb-b70b-e68fcf65d662)
    -

| **Field**                | **Description**                                              |
|--------------------------|--------------------------------------------------------------|
| **Pointer**               | Points to the next PCB in a queue or to related resources.   |
| **Process State**         | Current status of the process (Ready, Running, Waiting, etc.).|
| **Process Number (PID)**  | Unique identifier for each process.                          |
| **Program Counter (PC)**  | Address of the next instruction to be executed.              |
| **Register**              | Stores values of CPU registers for context switching.        |
| **Memory Limits**         | Defines the memory allocation (base address and limit).      |
| **List of Open Files**    | Contains file descriptors for files the process has opened.  |
- Context Switch:
  - Context Switch: Kernel saves the state of the old process and loads the saved state for the new process.
  - Context-switch time is purely overhead.
  - ![image](https://github.com/user-attachments/assets/ea68fd82-7169-465b-8893-ef31b5f2b642)
  - Switch time (about 1~1000ms) depends on:
    - memory speed
    - number of registers
    - existence of special instructions
    - hardware support 
# Process Schedling
- Multiprogramming: CPU runs process at all times to **maximize CPU utilization**.
- Time sharing: switch CPU frequently s.t. **users can interact** with each program while it is running.
- Processes will have to wait until the CPU is free and can be re-scheduled.
- Process schedling queues:
  - Processes migrate between the various queues.
  - **Job queue** (New state): set of all processes in the system.
  - **Ready queue** (Ready state): set of all processes residing in main memory, *ready and waiting to execute*.
  - **Device queue** (Wait State): set of processes *waiting for an I/O device*.
- Process scheduling diagram: ![image](https://github.com/user-attachments/assets/2376ca7e-3395-4fa7-970c-8a02c640757a)
- Schedulers:
  - **Short-term** scheduler (*CPU scheduler*): selects which process should be executed and **allocated CPU** (determine Ready state to Run state).
    - Execute quite frequently (milliseconds-level).
    - Must be efficient to reduce overhead.
  -**Medium-term** scheduler: selects which processes should be **swapped in/out memory** (determine Ready state to Wait state).
    - Improve process mix, free up memory.
    - Most modern OS skip the medium-term schedulers due to using virtual memory.   
  - **Long-term** scheduler (*job scheduler*): selects processes should be **loaded into memory** and brought into the ready queue (determine New state to Ready state).
    - Control **degree of multiprogramming**.
    - Select a **good mix of CPU-bound & I/O bound** processes to increase system overall performance.
    - Most modern OS skip the long-term schedulers due to having sufficient physical memory.   
# Process creation
- Resource sharing
  - Parent and child process share **all** resources.
  - Child process shares **subset** of parent's resources.
  - Parent and child share **no** resources. 
- Execution
  - Parent and child process **execute concurrently**.
  - Parent **wait until children terminate**.
- Address space
  - **Child duplicate of parent**, communication via sharing variables.
  - **Child has a program loaded into it**, communication via message passing.
- UNIX/Linux Process Creation
  - *fork()* system call:
    - Create a new (child) process.
    - The new (child) process **duplicates the address space** of its parent.
    - Child and Parent **execute concurrently** after fork.
    - Child: return value of *fork()* is 0.
    - Parent: return value of *fork()* is PID of the child process.
  - *execlp()* system call:
    - **Load a new binary file** into memory, **destroying the old code**. 
  - *wait()* system call:
    - The parent waits for **one of its child processes** to complete.   
# Interprocess Communication (IPC)
- IPC is a set of methods for the exchange of data among multiple threads in one or more processes.
  - Independent process: cannot be affect or be affected by other processes.
  - Cooperating process: O.W.
- Communication methods:
  - **Shared memory**:
    - Require more careful **user synchronization**.
    - Implement by memory access: faster speed.
    - **Use memory address to access data**.
  - **Message passing**:
    - No conflict: **more efficient for small data**.
    - **Use send/recv message**.
    - Implemented by **system call**: slower speed.
  - **Sockets**:
    - A network connection identified by **IP and port**.
    - Exchange **unstructured stream or bytes**.
  - **Remote procedure calls**:
    - Cause a **procedure** to execute in another address space.
    - Parameters and return values are passed by message.     
