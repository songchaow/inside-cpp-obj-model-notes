# Chapter 2
**Schwarz Error**

A type has defined a implicit type conversion to `int`, with the purpose of supporting scalar-test(标量测试，即判断该type为true或false). However, in some circumstances, unwanted conversions occur.
```c
cin << intval; // no error will print
```

Implicit class conversion is nevertheless important.

## Non-trivial default constructors
Those ctors that compiler needs. 4 situations:

- member class object with default ctor

- Base class with default ctor

- class with virtual functions

- class with virtual base class(es)
为什么虚bases类，需要在子类中 间接存储，现在更全面的认识到了：
  - 从子类访问父类的non-local variable，在编译时是无法确定偏移的。
  - 我之前想到的理由是，子类的方法需要调用虚基类的方法，在编译时是无法确定偏移量的。

  第一点其实更彻底。

  实现上的细节：A:X, B:X, C:A,B。X完全可以和ABC连续放一起，只不过A,B都存了指向C object地址的指针。

## Copy Ctor Construction

Two cases:
- Explicit initialization (copy initialization)
- When an object is passed as an argument
- When a function returns a class object

### Default Memberwise Initialization
Already known to who has read the *Primer*.
- for built-in and derived data members, copy the value directly.
- for memer class objects, apply memberwise initialization.

**Conceptually**, The above operation (of copying members) is implemented by copy ctors. But in reality, often bitwise copies are enough.

Whether a copy ctor  is trivial == whether the class exhibits **bitwise copy esmantics**.

Criterion of **NOT** copy semantics:

1.  the class contains a class member obj for which a copy ctor exists.
2. the class derived from a base class. and the base class's cp ctor exists.
3. the class declares one or more virtual functions.
  Problems arise when initializing a base class obj with a derived class object. The vptr should be set back to the one of the base class.
4. the class has virtual base classes.
  
    我的想法：如果bptr存储的是绝对值，那么每次拷贝，都不是bitwise semantics。如果存储的是虚基类与自己的相对偏移，则有时候可以是bitwise semantics。

    ~~现在悟出了一点：bptr应该存在于虚基类后面的每一个类对象的成员里。~~ bptr应该只有一个，所有后续的类都可以用同样的方式访问。所以应该在最开始的地方。

自己的总结。所以copy行为有三种可能：
1. class有bitwise copy semantics，直接内存数据复制。
2. class没有bitwise copy semantics，未定义cp ctor，编译器合成。此时该复制的都会复制。
3. class没有bitwise copy semantics，有定义cp ctor，此时只会复制ctor里有的成员，编译器**不会补充**。

## 2.3 Program Transformation Semantics

### Explicit Initialization
Two fold program transformations:
- Remove each definition's initialization.
- Add the class copy ctor.

### Argument Initialization
Two implementation strategies:
1. Construct a temporary obj at the calling side. And reference to that object in the function body.
2. Copy construct the obj directly at the argument's place on the activation record on the program stack. (Seems more intuitive)
### Return Value Initialization
Two fold transformation:
1. Add a reference argument of the returning type.

   The actual object of the argument is a temporary object, or the lvalue if the expression is a definition with a initializer.

2. Insert the initialization code of the reference argument.

**The above behaviour can be optimized to prevent copies**.

#### User Level Optimization
Return a computational ctor, and apply that ctor directly on the reference argument. Instead of creating a local variable and return it, which causes copying.

But adding those ctors merely for efficiency reasons can affect the original class design.
#### Compiler Level Optimization
NRV.


## 2.4 Memberwise Initialization List
