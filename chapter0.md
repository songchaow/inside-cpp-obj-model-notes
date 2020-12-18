# Preface

## Foundation project
The Foundation was an effort to
define a new development model for the construction of large systems

**Simplifier**

C++ object model part of Foundation Project.
A phase of compiler, between type checking and code generation. The simplifier doesthe following three conversions:
1. Implementation-dependent transformations

    ```c++
    fct()
    ```

2. Language semantics transformations
    > 将语言语意转换成针对对象模型的操作。转换后的操作仍然是在语言层面。

    constructors and destructors. memberwise initialization and memberwise copy.

3. Code and object model transformations (to codes)
    > 转换对象模型本身的操作。即对象模型的底层实现机制。

    vitual functions, vitual base class and inheritance, new and delete operators


C++ object model includes:
- language component that directly supports OO design.
- Low level mechanism that

## Types of C++ Object Model Low level Mechanism
- cfront program-based solutions
    
    由cfront实现品采用的方法。cfront是最早的编译器。这里重点是强调平台无关。
- Platform dependent solutions
    
    即平台相关