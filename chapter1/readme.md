# Chapter 1
## Whether to encapusulate?
## 需不需要对象封装
- From software engineering view, encapusulation is definitely better.
- Sometimes, raw C implementation(without encapusulation) is considered better, if programmers want an app up and running quickly.

### Layout **Runtime** Costs for Adding Encapsulation
Not at all!

## C++ Object Model

### Simple Object Model
The fields of an object are all pointers.
### A Table-driven Object Model
An object contains 2 pointer to 2 tables respectively.

**Member Data Table** stores actual member data.

**Function Member Table** stores function pointers.

### The C++ Object Model
The following statements are NOT complete.
- One class <=> one virtual table. The table contains all pointers to the class's virtual functions.
- Each class **object** stores a vptr, pointing to a virtual table. 
- Ctors, dctors, and cp assignemnt ops modify `vptr`.
- The virtual table also contains a pointer to `type_info` object. 