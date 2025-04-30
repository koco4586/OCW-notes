# Thread Review
- Lightweight process: basic unit of **CPU utilization**.
- Treads **belonging to the same process** share:
  - Code section.
  - Data section.
  - OS resources (open files, signals, etc.).
- Each thread has its own thread control block (TCB)
  - Thread ID.
  - Program counter.
  - Register set.
  - Stack.
- ![image](https://github.com/user-attachments/assets/f8462357-2370-4a4f-ae37-49d4e02e0f27)
# Benefits of Multithreading
- Responsiveness: allow a process to continue running even if part of it is blocked or is peforming a lengthy operation.
- Resoure sharing: several different threads of activity all within the **same address space**.
- Utilization of MP architecture: Several thread may be **running in parallel** on different processors.
- Economy: Allocating memory and resources for process creation is costly.
  - Lower creation cost vs. Process: ![image](https://github.com/user-attachments/assets/5bb7bb9a-8a83-4256-a44a-80327e42ef14)

  - Faster inter-process communication vs. MPI: ![image](https://github.com/user-attachments/assets/564c669a-46a9-484e-9dcf-ad7dd5456e05)
- Challenges in Multicore Programming
  - Dividing activities: divide program into concurrent tasks.
  - Data splitting: divide data accessed and manipulated by the tasks.
  - Data dependency: synchronize data access.
  - Balance: evenly distribute tasks to cores.
  - Testing and debugging.
# User vs. Kernel Threads
- User threads:
  - Thread management done by user-level threads library:
    - POSIX Pthreads.
    - Win32 threads.
    - Java threads.
  - **Thread library** provides support for thread creation, scheduling and deletion.
  - Generally **fast** to create and manage.
  - If the kernel is single-threaded, a user-thread blocks = entire process blocks even if other threads are ready to run. 
- Kernel threads:
  - Supported by the kernel (OS):
    - Windows 2000 (NT).
    - Solaris.
    - Linux.
    - Tru64 UNIX.
  - The **kernel** performs thread creation, scheduling, etc.
  - Generally **slower** to create and manage.
  - If a thread is blocked, the kernel can schedule another thread for execution.
- Multithreading Models:
  - Many-to-one:
    - Many user-level threads mapped to a single kernel thread.
    - Used on systems that do not support kernel threads.
    - Thread management is **done in user space**, so it is efficient.
    - Only one thread can access the kernel at a time, multiple threads are unable to run in parallel on multiprocessors. 
  - One-to-one:
    - There could be a limit on number of kernel threads.
    - More concurrency.
    - Overhead: Creating a thread requires creating the corresponding kernel thread. 
  - Many-to-many:
    - Many user-level threads to a smaller or equal number of kernel threads.
    - **Allows the developer to create as many user threads as wished**.
    - When a thread performs a blocking call, **the kernel can schedule another thread for execution**.     
# Shared-Memory Programming
- Definition: Processes communicationg or work together with each other **through a shared memory space** which can be accessed by all processes.
- Many issues:
  - **Synchronization**.
  - **Deadlock**.
  - **Cache coherence**.
- Programming techniques:
  - Parallel compiler.
  - Unix processes.
  - Threads (Pthread, Java).
- Pthread is the implementation of POSIX(Portable OS Interface) standard for thread.
  - pthread_create (thread,attr,routine,arg)
    - thread: An **unique identifier** for the new thread.
    - attr: It is used to set **thread attributes**. NULL for the default values.
    - routine: The routine that the thread will execute once it is created.
    - arg: A **single argument** that may be **passed to routine**.
  - Example:

    ```c
    #include <pthread.h>
    #include <stdio.h>
    #define NUM_THREADS 5

    void *PrintHello(void *threadId)
    {
      long* data = static_cast<long*>(threadId);
      printf("Hello World! It's me, thread #%ld!\n", *data);
      pthread_exit(NULL);
    }

    int main(int argc, char *argv[])
    {
      pthread_t threads[NUM_THREADS];
      for(long tid=0; tid<NUM_THREADS; tid++)
        {
        pthread_create(&threads[tid], NULL, PrintHello, (void *)&tid);
        }
      /* Last thing that main() should do */
      pthread_exit(NULL);
    }
  - pthread_join(threadid,status)
    - **Blocks until the specified threadid thread terminates**.
    - One way to accomplish synchronization between thread.
  - pthread_detach(threadid)
    - **Once a thread is detached, it can never be joined**.
    - Detach a thread could free some system resources.
- Linux Threads:
  - Linux does **not** support multithreading.
  - Various Pthreads implementation are available for **user-level**.
  - The *fork()* system call: create a new process and a **copy** of the associated data of the parent process.
  - The *clone()* system call: create a new process and a **link** that points to the associated data of the parent process.
    - A set of flags is used in the *clone()* call for indication of the level of the sharing.
-Signal handling:
  - Signal are used in UNIX systems to notify a process that an event occurs:
    - **Synchronous**: illegal memory access.
    - **Asynchronous**: <control-C>
  - A **signal handler** is used to process signals:
    1. Signal is generated by particular event.
    2. Signal is delivered to a process.
    3. Signal is handled.
  - Options:
    - Deliver the signal to the thread to which the signal applies.
    - Deliver the signal to every thread in the process.
    - Deliver the signal to certain threads in the process.
    - Assign a specific thread to receive all signals for the process.
- Thread Pools
  - Create a number of threads in a pool where they await work.
  - Advantages:
    - Usually slightly **faster to service a request** with an existing thread **than create a new thread**.
    - Allows the number of threads in the applications to be **bound to the size of the pool**.   
