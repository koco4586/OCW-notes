# Deadlock Problem
- A set of blocked processes each **holding** some resources and **waiting** to acquire a resource held by another process in the set.
- Necessary conditions (**all four conditions must hold for possible deadlock**):
  - **Mutual exclusion**: only 1 process at a time can use a resource.
  - **Hold & Wait**: a process holding some resources and is waiting for another resource.
  - **No preemption**: a resource can be only released by a process voluntarily.
  - **Circular wait**: there exists a set $\{ P_0, P_1, \dots, P_n \}$ of waiting processes such that  $P_0 \rightarrow P_1 \rightarrow \dots \rightarrow P_n \rightarrow P_0$.
- System Model:
  - Resources types: $R_1, R_2, \dots, R_m$.
  - Each resource type $R_i$ has **$W_i$ instances**.
  - Each process utilizes a resource as follows: Request $\rightarrow$ use $\rightarrow$ release.
  - Resource-Allocation Graph (RAG): ![image](https://github.com/user-attachments/assets/daa13b98-0153-4aec-a730-522d477b90dd)
    - Vertices (V): **processes** and **resources**.
    - Edges (E): **request edge** and **assignment edge**. 
    - Example (cycle with deadlock): ![image](https://github.com/user-attachments/assets/70f67cf8-613d-4b88-b091-fa957b9f2c86)
      - $P_1$ is waiting for $P_2$.
      - $P_2$ is waiting for $P_3$ ($P_1$ is also waiting for $P_3$).
      - Since $P_3$ is waiting for $P_1$ or $P_2$, and both $P_1, P_2$ waiting for $P_3$ $\rightarrow$ **deadlock!**.
    - Example (cycle without deadlock): ![image](https://github.com/user-attachments/assets/67d36783-1cef-4457-91cb-68080bbff481)
      - $P_1$ is waiting for $P_2$ or $P_3$.
      - $P_3$ is waiting for $P_1$ or $P_4$.
      - Since $P_2$ and $P_4$ wait no one $\rightarrow$ **no deadlock between $P_1$ and $P_3$**.
- Deadlock Detection:
  - If RAG contains **no cylce** $\rightarrow$ **no deadlock** since **circular wait** cannot be held.
  - If RAG contains a **cylce**:
    - If **one instance per resource type** $\rightarrow$ deadlock.
    - If **multiple instances per resource type** $\rightarrow$ **possibility** of deadlock.
# Handling Deadlocks
- Ensure the system will **never enter** a **deadlock state**:
  - Deadlock **prevention**: ensure that **at least one of the 4 necessary conditions** cannot held.
    - Mutual exclusion (ME): do not require ME on sharable resources.
      - No need to ensure ME on read-only files.
      - **Some resources are not shareable** (e.g. printer).
    - Hold & Wait:
      - When a process requests a resource, it does not hold any resource.
      - Pre-allocate all resources before executing.
      - **Resource utilization is low; starvation is possible**.
    - No preemption:
      - When a process is waiting on a resource, all its holding resources are preempted.
      - Applied to resources whose states can be easily saved and stored later (e.g. CPU registers & memory).
      - **It cannot easily be applied to some resources** (e.g. printer & tape drives).
    - Circular wait:
      - Impose a total ordering of all resources types.
      - A process requests resources in an increasing order:
        - Let $R$ = { $R_0, R_1, \dots, R_N$ } be the set of resource types.
        - **When request $R_k$, should release all $R_i, i\geq k$.**
  - Deadlock **avoidance**: **dynamically** examines the resource aloocation state before allocation.
    - Avoidance Algorithms:
      - **Single instance** of a resource type: **RAG algorithm** based on **cycle detection**.
        - Claim edge: $P_i \rightarrow R_j$, process $P_i$ may **request** $R_j$ **in the future**.
        - Claim edge converts to request edge if the resource is requested by process, algorithm decides whether the request edge can be safely converted into an assignment edge.
          - Resources **must be claimed a priori** in the system.
          - Grant a request only if **NO** cycle is created.
          - Check for safety using a cycle-detection algorithm ( $O(n^2)$ ).
      - **Multiple instances** of a resource type: **banker's algorithm** based on safe sequence detection.
        - Safe state: a system is in a safe state if there exists **a sequence of allocations** to satisfy requests by all processes, this sequence of allocations is called **safe sequence**.
        - Unsafe state: **possibility** of deadlock, to avoid deadlock is **to ensure the system never enters an unsafe state**.
        - Banker algorithm:
          - Use a general safety algorithm to **pre-determine** if any **safe sequence** exists after allocation.
          - Only proceed the allocation if safe sequence exists.
            - Safety algorithm:
              1. Assume processes need **maximum** resources.
              2. Find a process that can satisfied by free resources.
              3. Free the resource usage of the process.
              4. Repeat to step 2 until all processes are satisfied.
          - Example:
            - **Total instances:** A: 10, B: 5, C: 7
            - **Available instances:** A: 3, B: 3, C: 2
            - Process resources table:

| Process | Max A | Max B | Max C | Alloc A | Alloc B | Alloc C | Need A | Need B | Need C |
|---------|-------|-------|-------|---------|---------|---------|--------|--------|--------|
| P0      |   7   |   5   |   3   |    0    |    1    |    0    |   7    |   4    |   3    |
| P1      |   3   |   2   |   2   |    2    |    0    |    0    |   1    |   2    |   2    |
| P2      |   9   |   0   |   2   |    3    |    0    |    2    |   6    |   0    |   0    |
| P3      |   2   |   2   |   2   |    2    |    1    |    1    |   0    |   1    |   1    |
| P4      |   4   |   3   |   3   |    0    |    0    |    2    |   4    |   3    |   1    |       
            - Safe sequence: $P_1, P_3, P_4, P_2, P_0$
- Allow to **enter a deadlock state** and then **recover**:
  - Deadlock detection.
  - Deadlock recovery. 
