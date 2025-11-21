# Polymorphism in C++ — Complete Notes with Examples

Polymorphism is one of the most important concepts in Object-Oriented Programming.  
The word *polymorphism* means **“many forms”**, and in C++ it allows the same function name or operator to behave differently based on the context or object type.

---

## 📘 Table of Contents

1. [What Is Polymorphism?](#what-is-polymorphism)
2. [Types of Polymorphism](#types-of-polymorphism)
3. [Compile-Time Polymorphism](#compile-time-polymorphism)
   - [Function Overloading](#Function-Overloading)
   - [Operator Overloading](#Operator-Overloading)
4. [Run-Time Polymorphism](#run-time-polymorphism)
   - [Function Overriding](#function-overriding)
   - [How Runtime Polymorphism Works](#how-runtime-polymorphism-works)
5. [Difference Between Compile-Time and Run-Time Polymorphism](#difference-between-compile-time-and-run-time-polymorphism)
6. [Why Polymorphism Is Important](#why-polymorphism-is-important)
7. [Short Exam-Friendly Definitions](#short-exam-friendly-definitions)
8. [Small Example Programs](#small-example-programs)
9. [Final Summary](#final-summary)

---

# What Is Polymorphism?

Polymorphism allows the same function name or operator to behave differently based on the object or data type involved.

C++ supports two kinds of polymorphism:

- **Compile-Time Polymorphism**
- **Run-Time Polymorphism**

---

# Types of Polymorphism

| Type | Achieved Through | When Resolved |
|------|------------------|----------------|
| **Compile-Time Polymorphism** | Function Overloading, Operator Overloading | During Compilation |
| **Run-Time Polymorphism** | Virtual Functions, Function Overriding | During Program Execution |

---

# Compile-Time Polymorphism

Compile-time polymorphism is also known as:

- **Early Binding**
- **Static Polymorphism**

Here, the compiler decides which function/operator to call during compilation.

C++ supports compile-time polymorphism via:

- Function Overloading  
- Operator Overloading  

---

## Function Overloading

Function overloading means having **multiple functions with the same name**, but different parameter lists.

A function can be overloaded by:

- Changing the *number* of parameters  
- Changing the *type* of parameters  

### ✔ Example

```cpp
#include <iostream>
using namespace std;

void print(int x) { cout << "Integer: " << x << endl; }
void print(double y) { cout << "Double: " << y << endl; }
void print(string s) { cout << "String: " << s << endl; }

int main() {
    print(10);
    print(3.14);
    print("Hello");
}
```

---

## Operator Overloading

Operator overloading allows C++ operators to behave differently with **user-defined types**.

Why operator overloading?

* Code becomes readable
* Natural mathematical expressions (e.g., `c1 + c2`)
* Custom behavior for objects

### ✔ Example — Overloading `+`

```cpp
#include <iostream>
using namespace std;

class Complex {
public:
    int real, imag;

    Complex(int r = 0, int i = 0) : real(r), imag(i) {}

    Complex operator+(const Complex& c) {
        return Complex(real + c.real, imag + c.imag);
    }
};

int main() {
    Complex c1(4, 5), c2(1, 2);
    Complex c3 = c1 + c2;

    cout << "Result: " << c3.real << " + " << c3.imag << "i";
}
```

---

# Run-Time Polymorphism

Run-time polymorphism is also called:

* **Late Binding**
* **Dynamic Polymorphism**

The function call is resolved **during execution**, based on the actual object.

Achieved using:

* Inheritance
* Virtual Functions
* Function Overriding
* Base class pointers/references

---

## Function Overriding

Function overriding occurs when a **derived class** provides its own implementation for a function already defined in the **base class**.

### ✔ Example

```cpp
#include <iostream>
using namespace std;

class Base {
public:
    virtual void show() { 
        cout << "Base class" << endl; 
    }
};

class Derived : public Base {
public:
    void show() override { 
        cout << "Derived class" << endl; 
    }
};

int main() {
    Base* b;
    Derived d;

    b = &d;    
    b->show();  // Calls Derived::show()
}
```

---

# How Runtime Polymorphism Works

Runtime polymorphism internally uses:

* **vtable (Virtual Table)**
* **vptr (Virtual Pointer)**

When a class has at least one `virtual` function:

* The compiler creates a vtable for it.
* Each object gets a hidden `vptr` pointing to its class’s vtable.
* Calls like `ptr->show()` use the vtable to find the correct function at runtime.

---

# Difference Between Compile-Time and Run-Time Polymorphism

| Feature        | Compile-Time Polymorphism | Run-Time Polymorphism          |
| -------------- | ------------------------- | ------------------------------ |
| Binding        | Early                     | Late                           |
| When resolved  | Compile time              | Runtime                        |
| Achieved using | Overloading               | Overriding + Virtual Functions |
| Keyword        | None                      | `virtual`                      |
| Speed          | Faster                    | Slightly slower                |
| Flexibility    | Less                      | More                           |

---

# Why Polymorphism Is Important

Polymorphism helps in:

* Flexible and reusable code
* Implementing OOP effectively
* Writing generic interfaces
* Modeling real-world relationships
* Extending code without modifying existing classes

### Real-world example:

```cpp
Shape* s;

s = new Circle();
s->draw();    // Circle's implementation

s = new Rectangle();
s->draw();    // Rectangle's implementation
```

---

# Short Exam-Friendly Definitions

* **Polymorphism:** Ability of a function/operator to take multiple forms.
* **Compile-Time Polymorphism:** Achieved using function/operator overloading.
* **Run-Time Polymorphism:** Achieved using overriding + virtual functions.
* **Function Overloading:** Same function name, different parameter list.
* **Function Overriding:** Derived class redefines base class method.
* **Virtual Function:** Function enabling run-time polymorphism.

---

# Small Example Programs

---

### ✔ Program 1 — Function Overloading

```cpp
#include <iostream>
using namespace std;

class Math {
public:
    int add(int a, int b) { return a + b; }
    double add(double a, double b) { return a + b; }
};

int main() {
    Math m;
    cout << m.add(5, 3) << endl;
    cout << m.add(2.5, 4.5) << endl;
}
```

---

### ✔ Program 2 — Operator Overloading (`+`)

```cpp
#include <iostream>
using namespace std;

class Number {
public:
    int value;

    Number(int v) : value(v) {}

    Number operator+(const Number& n) {
        return Number(value + n.value);
    }
};

int main() {
    Number n1(10), n2(20);
    Number n3 = n1 + n2;

    cout << "Sum: " << n3.value;
}
```

---

### ✔ Program 3 — Runtime Polymorphism Using Virtual Functions

```cpp
#include <iostream>
using namespace std;

class Animal {
public:
    virtual void sound() {
        cout << "Animal makes a sound" << endl;
    }
};

class Dog : public Animal {
public:
    void sound() override {
        cout << "Dog barks" << endl;
    }
};

int main() {
    Animal* a;
    Dog d;

    a = &d;
    a->sound(); // Calls Dog::sound()
}
```

---

# Final Summary

Polymorphism is essential to achieving:

* Extensibility
* Reusability
* Cleaner OOP design
* Flexible function/behavior selection

C++ supports two major types:

* **Compile-Time Polymorphism** → Overloading
* **Run-Time Polymorphism** → Virtual functions + overriding

Polymorphism helps build modular, scalable, real-world C++ applications.

---

