# Basic Concepts
- The idea of multiprogramming:
  - Keep several processes in memory. Every time one process has to wait, another process takes over the use of the CPU.
- **CPU-I/O burst cycle**: Process execution consists of a cycle of CPU execution and I/O wait.
  - Generally, there is a large number of short CPU bursts, and a small number of long CPU bursts.
  - A I/O-bound program would typically has meny very short CPU bursts.
  - A CPU-bound program might have a few long CPU bursts.
  - ![image](https://github.com/user-attachments/assets/0163125b-b21b-420b-a27c-386b5ee7205f)
- CPU scheduler:
  - Selects from ready queue to execute.
  - CPU scheduling decisions may take place when a process:
    1. Switches from running to waiting state.
    2. Switches from running to read state.
    3. Switches from waiting to ready.
    4. Terminates.
  - Non-preemptive scheduling:
    - Scheduling under (a) and (d).
    - The process keeps the CPU until it is **terminated** or **switched to the waiting state**.
  - Preemptive scheduling:
    - Scheduling under all cases.
  - Preemptive Issues:
    - Inconsistent state of shared data:
      - Require **process synchronization**.
      - Incurs a cost associated with access to shared data.
    - Affect the design of **OS kernel**:
      - UNIX solution: **waiting** either for a system call to complete or for an I/O block to take place before doing a context switch (disable interrupt).
- Dispatcher:
  - Dispatcher module gives control of the CPU to the process selected by scheduler:
    - Context-switch
    - Jumping to the proper location in the selected program.
  - Dispatch latency: time it takes for the dispatcher to stop one process and start another running:
    - Scheduling time.
    - Interrupt re-enabling time.
    - Context-switch time.      
