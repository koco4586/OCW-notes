# OS services
- **User interface**
- Program Execution
- I/O operations
- File-system manipulation
- **Communication**
- Error detection
- *Resource allocation*
- *Accounting*
- *Protection and security*
# System Calls
- Request OS services:
  - **Process control**: abort, create, terminate process, allocate/free memory.
  - **File management**: create, delete, open, close file.
  - **Device management**: read, write, reposition device.
  - **Information maintenance**: get time or date.
  - **Communications**: send or receive message.
- The **OS interface** to a running program.
- An explicit request to the **kernel** mode via a **software interrupt**.
- Generally available as **assembly-language** instructions.
# Application Program Interface (API)
- *Users mostly program against API instead of system call.*
- Commonly implemented by language libraries, e.g. **C Library**.
- An API call could involve *zero or multiple system call*.
  - Both *malloc()* and *free()* use system call *brk()*.
  - Math API functions, such as *abs()*, do not need to invovle system call.
- ![image](https://github.com/user-attachments/assets/c8c352b3-ca8f-4e37-ac3e-f38d39f727e8)
- Three most common APIs:
  - **Win API** for Windows.
  - **Portable Operating System Interface for Unix (POSIX) API** for UNIX, Linux and Mac OS.
  - **Java API** for the Java virtual machine.
- API - Systemcall - OS relationship: ![image](https://github.com/user-attachments/assets/ed07e4da-e27f-4dd3-9162-08fc02819eb0)
- Why use API?
  - Simplicity
    - API is designed for applications.
  - Portability
    - API is an unified defined interface.
  - **Efficiency**
    - Not all functions require OS services or involve kernel.
- Three general methods for passing parameters between a running program and the OS:
  - Pass the parameters in registers
(this may prove insufficient when there are more parameters than registers).
  - Store the parameters in a table in memory, and pass the table address as a parameter (pointer) in a register.
  -  Push the parameters onto a stack; to be popped off by the OS. Block and stack
methods do not limitt the number or length of parameters passed.
# Layered OS Architecture
- Lower levels independent of upper levels.
  - N<sup>th</sup> layer can only acess services provided by 0 ~ (N-1)<sup>th</sup> layer.
- ![image](https://github.com/user-attachments/assets/a42d9af6-0103-446e-a201-c43f7624a632)
- pros:
  - Modularity
  - Easy debugging/updating
  - No direct access to hardware (Safety)
  - Abstraction
- cons:
  - less efficient
  - difficult to define layers
# Microkernel OS
- Move as much from kernel into **user space**.
- Communication is provided by **message passing**.
- ![image](https://github.com/user-attachments/assets/1fd29e5d-0971-4d41-a9c8-de616098d9d2)
- Easier to extend and port, but it performs more slowly due to communication costs.
# Modular OS Architecture
- Most modern OS implement **kernel modules**.
  - Uses **object-oriented approach**.
  - Each core **component is separate**.
  - Communicates via **known interfaces**.
  - Each is loadable as needed within the kernel.
- Similar to layers but with more flexible
- ![image](https://github.com/user-attachments/assets/e9988b67-ed7d-486d-973f-9672e9239325)
