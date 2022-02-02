# object-oriented programming language

### 1. what is oop

Object-oriented programing is a program paradigm that is resolve around the concept of **object**

### 2. what is object

Objects can be considered as real-world instances of entities like class, it contains some charateristic and its specified behavior.

e.g. car class

charaeristic = type/brand/etc
behavior = start/stop/etc

### 3. What is the need for OOPs?

1. OOPs helps users to understand the software easily, although they don’t know the actual implementation.
2. OOPs can increase **readability, understandability, and maintainability**.

### 4. 有什么其他的 programming paradigm 么

1. Imperative Programming Paradigm 命令式
2. Declarative Programming Paradigm

### 5. what is the main feature of OOP

#### 1. inheritence

`class circle extends shape`

The term “inheritance” means “**receiving some quality or behavior from a parent to an offspring.**” In object-oriented programming, inheritance is the mechanism by which an object or class (referred to as a child) is created using the definition of another object or class (referred to as a parent).

- keep the implementation simpler
- facilitate 推动 code reuse

#### 1.1 Are there any limitations of Inheritance?

1. Inheritance needs more time to process, as it needs to navigate through multiple classes for its implementation.
2. The base class and the child class, are very tightly coupled together. So if one needs to make some changes, they might need to do nested changes in both classes.
3. More complex implementation, may lead to unexpected error/output if not implement correctly

#### 2. encapsulation

What it means is that by Encapsulation, all the necessary data and methods are bind together and all the unnecessary details are hidden to the normal user. So Encapsulation is the process of binding data members and methods of a program together to do a specific job, without revealing
unnecessary details.

1.  Data hiding: Encapsulation is the process of hiding unwanted information, such as restricting access to any member of an object.

2.  Data binding: Encapsulation is the process of binding the data members and the methods together as a whole, as a class.

#### 3. polymorphism

Polymorphism refers to the process by which some code, data, method, or object **behaves differently under different circumstances or contexts.** Compile-time polymorphism and Run time polymorphism are the two types of polymorphisms in OOPs languages.

- Compile Time Polymorphism: Compile time polymorphism, also known as Static Polymorphism, refers to the type of Polymorphism that happens at compile time. What it means is that the compiler decides what shape or value has to be taken by the entity in the picture.

- Runtime Polymorphism: Runtime polymorphism, also known as Dynamic Polymorphism, refers to the type of Polymorphism that happens at the run time. What it means is it can't be decided by the compiler. Therefore what shape or value has to be taken depends upon the execution.

#### 4. abstraction

If you are a user, and you have a problem statement, you don't want to know how the components of the software work, or how it's made. You only want to know how the software solves your problem. Abstraction is the method of hiding unnecessary details.
For example, consider a car. You only need to know how to run a car, and not how the wires are connected inside it. This is obtained using Abstraction.

### 6. How much memory does a class occupy?

Classes do not consume any memory. They are just a blueprint based on which objects are created. Now when objects are created, they actually initialize the class members and methods and therefore consume memory.

### 7. Is it always necessary to create objects from class?

Not necessary for static method. You can directly call static method but for non-static you need to created an object first to call it.

> Static methods are the methods in Java that can be called without creating an object of class. They are referenced by the class name itself or reference to the Object of that class.

### 8. What is a constructor?

Constructors are special methods whose name is the same as the class name. The constructors serve the special purpose of initializing the objects.
For example, suppose there is a class with the name “MyClass”, then when you instantiate this class, you pass the syntax:
MyClass myClassObject = new MyClass();

### 9. different type of constructor in java

- No-Arg Constructor.
- Parameterized Constructor.
- Default Constructor.

### 10. What is a destructor?

Contrary to constructors, which initialize objects and specify space for them, Destructors are also special methods. But destructors free up the resources and memory occupied by an object. Destructors are automatically called when an object is being destroyed.

### 11. Are class and structure the same? If not, what's the difference between a class and a structure?

No, class and structure are not the same. Though they appear to be similar, they have differences that make them apart. For example, **the structure is saved in the stack memory, whereas the class is saved in the heap memory.** Also, **Data Abstraction cannot be achieved with the help of structure**, but with class, Abstraction is majorly used.

### 12. what is abstract class / abstract method ?

随着继承层次中ー个个新子类的定义, 类变得越来越具体, 而父类则更 general。类的设计应该保证父类和子类能够共享特征。有时将一个父类设计得非常抽象,以至于它没有具体的实例,这样的类叫做**抽象类**。

**Data abstraction is the process of hiding certain details and showing only essential information to the user.**

1. Abstraction can be achieved with either **abstract classes or interfaces**

2. The `abstract` keyword is a non-access modifier, used for classes and methods:

3. **Abstract class**: is a restricted class that cannot be used to create objects / They can not be instantiated (**to access it, it must be inherited from another class**). Abstract class is like a prototype, only method declaration, no method body. It must contain abstract method, but it can also have non-abstract method

4. **Abstract method**: can only be used in an abstract class, and it does not have a body. The body is provided by the subclass (inherited from).

5. An abstract class can have both abstract and regular methods:

   ```java
   abstract class Animal {
    public abstract void animalSound();
    public void sleep() {
        System.out.println("Zzz");
    }
   }
   ```

6. From the example above, it is not possible to create an object of the Animal class:

   ```java
   Animal myObj = new Animal(); // will generate an error
   ```

### 13. what is interface?

https://docs.oracle.com/javase/tutorial/java/IandI/createinterface.html

Interface is also a way to achieve abstraction like abstract class, which group related methods with empty bodies.

There are a number of situations in software engineering when it is important for disparate groups of programmers to agree to a "contract" that spells out how their software interacts. Each group should be able to write their code without any knowledge of how the other group's code is written. Generally speaking, interfaces are such contracts.

In the Java programming language, an interface is a reference type, similar to a class, that can contain only constants, method signatures, default methods, static methods, and nested types. Method bodies exist only for default methods and static methods. **Interfaces cannot be instantiated — they can only be implemented by classes or extended by other interfaces.**

### 14. abstract class vs interface

https://www.youtube.com/watch?v=Lnqmde9LP74

1. The biggiest different between these two is purpose:

   - **abstract class is used when you want to generalize behavior**
   - **interface is used to when you want to standardize behavior**

   if you have multiple implementations and you see a pattern across them and you want to generalize it, in a inheritance hierachy. You can create a abstract class and put the general behavior at the top.

   As for interface, it is like a contract and every implementation of an interface has to follow/adhere the contract.

2. so it comes to the next question - **why a class inheritance extending from mutiple classes but can implemented multiple interface?**

   It is because if two parent class has the same method, we can not decide which one to take if we extend multiple classes. However, if we implemented multiple interface it is ok because it is means our implementation adheres/follows both standard.

## 参考

https://www.interviewbit.com/oops-interview-questions/
