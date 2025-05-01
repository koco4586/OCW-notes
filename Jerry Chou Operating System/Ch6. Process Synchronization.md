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
# Pthrea Lock/Mutex Routines
- To use mutex, it muse be declared as of **type** `pthread_mutex_t` and initialized with `pthread_mutex_init()`.
- A mutex is destroyed with `pthread_mutex_destroy()`.
- A critical section can then be protected using `pthread_mutex_lock()` and `pthread_mutex_unlock()`.
- Example:

  ```c
  #include "pthread.h"
  pthread_mutex_t mutex;
  pthread_mutex_init(&mutex, NULL);    // specify default attribute for the mutex
  pthread_mutex_lock(&mutex);          // enter critical section

  // Critical Section

  pthread_mutex_unlock(&mutex);        // leave critical section
  pthread_mutex_destroy(&mutex);
- Condition Variables (CV)
  - CV represent some **condition** that a thread can:
    - Wait on, until the condition occurs.
    - Notify other waiting threads that the condition has occurred.
  - Three operations on CV:
    - `wait()`: **Block** until another thread calls `signal()` or `broadcast()` on the CV.
    - `signal()`: Wake up **one thread** waiting on the CV. 
    - `broadcast()`: Wake up **all threads** waiting on the CV.
  - In pthread, CV type is a **pthread_cond_t**:
    - Use `pthread_cond_init()` to initialize.
    - `pthread_cond_wait(&theCV,&somelock)`.       
    - `pthread_cond_signal(&theCV)`.
    - `pthread_cond_broadcast(&theCV)`.
  - All CV operation **MUST be performed while a mutex is locked**.
# Hardware Support
- The CS problem occurs because the modification of a shared variable may be **interrupted**.
- HW support solution: atomic instructions
  - Atomic: **as one uninterruptible unit**.   
  - Examples: `TestAndSet(var)`, `Swap(a,b)`.
    - `TestAndSet(var)`:

    ```c
    boolean TestAndSet(boolean &lock)
    {
      boolean value = lock;
      lock = TRUE;
      return value;
    }
    // This function executes atomically:
    // returns the original value of "lock" and sets "lock" to TRUE
    ```
      - $P_{0}$:

        ```c
        // Shared data:boolean lock; initially lock=False
      

        do { // P0
            while (TestAndSet(lock));     // wait until lock is free

            // critical section

            lock = FALSE;                 // release the lock

            // remainder section
        } while (1);
        ```
      - $P_{1}$:
  
        ```c
        do { // P1
            while (TestAndSet(lock));     // obtain lock
  
            // critical section

            lock = FALSE;                 // release lock

            // remainder section
        } while (1);

        ```
    - `Swap(a,b)`:
      - $P_{0}$:

        ```c
        // idea: enter CS if lock == false
        // Shared data:boolean lock; initially lock=False
      

        do { // P0
            key0 = TRUE;
            while (key0 == TRUE)     
              Swap(lock,key0);
            // critical section
            lock = FALSE;               
            // remainder section
        } while (1);
        ```
      - $P_{1}$:

        ```c
        do { // P1
            key1 = TRUE;
            while (key1 == TRUE)     
              Swap(lock,key1);
            // critical section
            lock = FALSE;               
            // remainder section
        } while (1);
        ```
# Semaphore
- A tool to generalize the synchronization problem (easier to solve, bute noo guarantee for correctness)
- A record of **how many units** of a particular resource are available
  - If #record = 1, it is called **binary semaphore** or **mutex lock**.
  - If #record > 1, it is called **counting semaphore**.
- Accessed only through 2 **atomic** ops: *wait* & *signal*.
- Semaphore is a part of **POSIX standard** BUT it is **not belonged to Pthread** (it can be used with or without thread).
  - POSIX Semaphore routines:
    - `sem_init(sem_t *sem, int pshared, unsigned int value)`    (unsigned int value = Initial value of the semaphore)
    - `sem_wait(sem_t *sem)`
    - `sem_post(sem_t *sem)`
    - `sem_getvalue(sem_t *sem, int *valptr)`  (int *valptr = current value of the semaphore)
    - `sem_destroy(sem_t *sem)`

