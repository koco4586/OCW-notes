# System life cycle  
1. Requirements:
   
   The information about inputs and outputs is described in the requirements. Frequently the initial specifications are defined vaguely, and we must develop rigorous input and output descriptions that include all cases.
2. Analysis:
    - Top-down:

      Start with the highest-level design and then break it down into smaller components or modules.
        - Clear Structure.
        - Easier for Planning.
        - Reduced Redundancy.
    - Bottom-up.
3. Design:

    The creation of abstract data type (ADT) and specification of algorithms.
4. Refinement and coding.
5. Verification.
# Algorithm criteria (Knuth's characterization)

   Definition: An algorithm is a finite sequence of mathematically rigorous instructions, typically used to solve a class of specific problems or to perform a computation.
1. Finiteness: An algorithm must always terminate after a finite number of steps.
2. Definiteness: Each step of an algorithm must be precisely defined; the actions to be carried out must be rigorously and unambiguously specified for each case.
3. Input: Quantities which are given to it initially before the algorithm begins. These inputs are taken from specified sets of objects.
4. Output: Quantities which have a specified relation to the inputs.
5. Effectiveness: All of the operations to be performed in the algorithm must be sufficiently basic that they can in principle be done exactly and in a finite length of time by a man using paper and pencil.
# Object-Oriented Programming

**Definition**: An *object* is an entity that performs computations and has a local state. It may therefore be viewed as a combination of data and procedural elements. 

**Definition**: *Object-oriented programming* is a method of implementation in which:
1. Objects are the fundamental building blocks.
2. Each object is an instance of some types (or class).
3. Classes are related to each other by inheritance relationships.

**Definition**: A language is said to be an *object-oriented language* if:
1. It supports objects.
2. It requires objects to belong to a class.
3. It supports inheritance.

A language that supports the first two features but does not support inheritance, is an *object-based* language. C++ is an object-oriented language while JavaScript is an object-based language.
# Common C++ Primitive Data Types
| Data Type               | Size (bytes) | Value Range                                      | Notes                                     |
|-------------------------|--------------|--------------------------------------------------|-------------------------------------------|
| `bool`                  | 1            | `true` or `false`                                | Stored as 1 byte                          |
| `char`                  | 1            | -128 to 127 **or** 0 to 255     (ASCII)          | Signedness is implementation-defined      |
| `int`                   | 4            | –2,147,483,648 to 2,147,483,647                  | 32-bit signed integer                     |
| `unsigned int`          | 4            | 0 to 4,294,967,295                               | 32-bit unsigned integer                   |
| `long long`             | 8            | –9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 | Guaranteed 64-bit signed integer  |
| `unsigned long long`    | 8            | 0 to 18,446,744,073,709,551,615                  | 64-bit unsigned integer                   |
| `float`                 | 4            | ±3.4×10³⁸ (6–7 decimal digits precision)         | IEEE 754 single precision                 |
| `double`                | 8            | ±1.8×10³⁰⁸ (15 decimal digits precision)         | IEEE 754 double precision                 |
