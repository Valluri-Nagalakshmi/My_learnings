# Polymorphism in C++ — Complete Notes with Examples

Polymorphism is one of the most important concepts in Object-Oriented Programming.  
The word *polymorphism* means **“many forms”**, and in C++ it allows the same function name or operator to behave differently based on the context or object type.

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

###  Example

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

###  Example — Overloading `+`

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

###  Example

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

###  Program 1 — Function Overloading

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

###  Program 2 — Operator Overloading (`+`)

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

###  Program 3 — Runtime Polymorphism Using Virtual Functions

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

# Polymorphism Interview Questions

## 1. What is polymorphism in C++?
Polymorphism is the ability of a function, method, or operator to behave differently based on the object or data type it is working on.

---

## 2. What are the two types of polymorphism in C++?
- Compile-Time Polymorphism  
- Run-Time Polymorphism  

---

## 3. What is the difference between compile-time and run-time polymorphism?
- **Compile-time polymorphism:** Resolved by the compiler (function overloading, operator overloading).  
- **Run-time polymorphism:** Resolved at runtime using virtual functions and overriding.

---

## 4. What is function overloading?
Function overloading means creating multiple functions with the same name but different parameter types or counts.

---

## 5. What is function overriding?
When a derived class provides a new definition of a function already defined in the base class.

---

## 6. What is a virtual function?
A function declared in the base class using the `virtual` keyword that supports run-time polymorphism.

---

## 7. Why do we use the `virtual` keyword?
To ensure the correct overridden function (derived class version) is called during runtime using base class pointers.

---

## 8. Can we achieve runtime polymorphism without virtual functions?
No. Without `virtual`, the base class version gets called due to static binding.

---

## 9. What is a pure virtual function?
A function declared with `= 0` in the base class, which forces derived classes to override it.

---

## 10. What is an abstract class?
A class containing at least one pure virtual function. Objects of an abstract class cannot be created.

---

## 11. What is a vtable and vptr?
- **vtable:** A table of pointers to virtual functions.  
- **vptr:** A hidden pointer inside each object that points to its class’s vtable.

---

## 12. What is the difference between overloading and overriding?
- **Overloading:** Same name, different parameters (compile-time).  
- **Overriding:** Same name, same parameters (run-time).

---

## 13. Can constructors be virtual? Why?
No. Virtual dispatch works only on objects already created.

---

## 14. Can destructors be virtual? Why?
Yes. Virtual destructors ensure proper cleanup when deleting objects through base class pointers.

---

## 15. Can we overload operators in C++? Does it relate to polymorphism?
Yes, operator overloading is a form of **compile-time polymorphism**.

---

## 16. What happens if we don’t declare a base class function as virtual?
The function call will bind statically, calling the base version always — preventing runtime polymorphism.

---

## 17. What is dynamic binding?
The process of resolving function calls at runtime based on the actual object type (via virtual functions).

---

## 18. What is method hiding in C++?
If a derived class function has the same name but different parameters as a base class function, the base function gets hidden.

---

## 19. Can polymorphism occur without inheritance?
- **Compile-time polymorphism:** Yes (overloading).  
- **Run-time polymorphism:** No, inheritance and virtual functions are required.

---



