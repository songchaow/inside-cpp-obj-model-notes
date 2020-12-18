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

## Inheritance
Following approaches are **not** adopted by C++:
- Using "Simple Object Model", includes addresses of all base class **objects**. Or use a `bptr`, similar to the virtual table, containing all addresses.
    
	The advantage: all class objects(various types) behave the same in terms of inheritance. (The `bpr` location is the same). The size of objects is not easily changed.

### C++ Inheritance Model
Place base class subobjects directly in derived class objects.

**Virtual base class**

理解：
考虑`A: virtual B`, `C: virtual B`。`D1: A,C`, `D2: C,...`
Virtual base class B在derived class D1,D2中的位置待定，可能在不同的derived class中被放到不同位置。由于virtual base class的各个方法本身需要编译与唯一确定，这些方法对base class中non static成员的访问方式也需唯一确定。

如果在A,C的成员函数中的对B的成员函数的调用处，就将对B的函数调用时，B在对象内的偏移量给写死了，就没办法应对B的位置不同的问题了。

所以还是得使用间接的方式，即在A和C中加入一个指针，指向virtual base **class**。

## Keyword differences in C and C++
Mainly on `struct` and `class`.

Do not use the following trick in C++:
```c
struct mumble {
	/*stuff*/
	char pc[1];
};
```
Then malloc a big space for a `mumble` object, and assign to the last memory part (from `&pc`)

**Possible approach(Deprecated now)**

Let C++ class inherits from a C struct, if one wants some data members' declaration to resemble C style (compatible with C memory layout).

**Best Approach**

Use composition instead of inheritance. 使用组合而不是继承。

## 1.3 An Object Distinction
### 3 Programming paradigms C++ supports
- procedural model
- abstract data type model
- object-oriented model