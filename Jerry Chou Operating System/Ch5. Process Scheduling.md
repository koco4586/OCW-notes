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
# Scheduling Criteria
- CPU utilization:
  - Theoretically:0% ~ 100% / real systems: 40% (light) ~ 90% (heavy).
- Throughput:
  - Number of completed **processes per time unit**.
- **Turnaround time**:
  - Time of submission to completion.
- **Waiting time**:
  - Total waiting time in the **ready queue**.
- **Response time**:
  - Time of submission to the **first response** is produced.
# Scheduling algorithms  
- First-come-first-serve (FCFS) Scheduling:
  - Example: ![image](https://github.com/user-attachments/assets/d8ad5da0-be2d-49ac-9ba4-ceb9e8d128db)
- Shortest-Job-First (SJF) Scheduling:
  - Associate with each process the length of its next CPU burst.
  - **A process with shortest burst length gets the CPU first** .
  - **SJF provides the minimum average waiting time**.
  - Two schemes:
    - Non-preemptive: once CPU given to a process, it cannot be preempted until its completion.
      - Example: ![image](https://github.com/user-attachments/assets/3cc22c8b-2a18-4b9f-b25d-fc15faea3ece)
    - Preemptive: if a new process arrives with shorter burst time length, preemption happens.
      - Example: ![image](https://github.com/user-attachments/assets/0491171b-9a36-49b1-8e3d-a4b131e866cc)
- Approximate-Shortest-Job-First Scheduling:
  - SJF difficulty: **no way to know length of the next CPU burst**.
  - Approximate SJF: the next burst can be **perdicted** as an exponential average of the measured length of previous CPU bursts.   
- Priority Scheduling:
  - A priority number is associated with each process.
  - **The CPU is allocated to the highest priority process**.
  - SJF is a priority scheduling where priority is the predicted next CPU burst time.
  - Problem: starvation (low priority processes never execute).
  - Solution: aging (as time increases, increase the priority of process).
