# Background
- **Concurrent** access to shared data may result in **data inconsistency**.
- Maintaining data consistency requires mechanism to **ensure the orderly execution** of cooperating processes.
- **Race Condition**: the situation where several processes **access and manipulate shared data concurrently**. The final value of the shared data **depends upon which process finishes last**.
  - To prevent race condition, **concurrent** processes must be **synchronized**.
  - On a single-processor machine, we could **disable interrupt** or use **non-preemptive CPU scheduling** (Only for OS kernel).
  - Commonly described as **critical section problem**.
- The Critical-Section Problem:
  - Purpose: a protocol for processes to cooperate.
  - Problem description:
    - **N processes** are competing to use some **shared data**.
    - Each process **has a code segment**, called **critical section**(CS), in which the shared data is accessed.
    - Ensure that when one process is executing in its critical section, **no other process is allowed to execute in its critical section** = **mutually exclusive**.
  - Requirements:
    - **Mutual Exclusion**: If process P is executing in its CS, no other processes can be executing in their CS.
    - **Progress**: If no process is executing in its CS and there exist some processes that wish to exter their CS, these **processes cannot be postponed indefinitely**.
    - **Bounded waiting**: A **bound must exist** on the number of times that **other processes are allowed to enter their CS** after a process has made a request to enter its CS.
  - Algorithm for two processes:
    - Peterson's Solution:
      - Only two processes, $P_{0}$ and $P_{1}$.
      - Shared variables:
        - int turn (initially turn=0);
        - turn = i means $P_{i}$ can enter its CS.
        - boolean **flag[i]** (initially flag[0]=flag[1]=false).
        - flag[i]=ture means $P_{i}$ is ready to enter its critical section.
      - Proof:
        - Mutual exclusion: ![image](https://github.com/user-attachments/assets/27ca05cf-ba39-4457-b84b-117135a2520c)
        - Progress: ![image](https://github.com/user-attachments/assets/0e3e4124-39db-4c93-84d4-78f6007e4f98)
        - Bounded waiting: ![image](https://github.com/user-attachments/assets/d4920b80-7464-433d-8e3d-9033fbef85fc)
    - Bakery Algorithm (n process):
      - Before enter its CS, each **process receives a #**.
      - Holder of the **smallest # enters CS**.
      - The numbering sheme always **generates # in non-decreasing order**.
      - If processes $P_{i}$ and $P_{j}$ receive the same #, if $i<j$, then $P_{i}$ is served first.
      - Example:

        ```c
        // Process i:
        do {
            // Get ticket
            choosing[i] = TRUE;
            num[i] = max(num[0], num[1], ..., num[n-1]) + 1;
            choosing[i] = FALSE;

            // FCFS
            for (j = 0; j < n; j++) {
                while (choosing[j]); // Cannot compare when num is being modified
                while ((num[j] != 0) && ((num[j], j) < (num[i], i)));
            }

            // critical section

            // release ticket
            num[i] = 0;

            // reminder section
        } while (1);


