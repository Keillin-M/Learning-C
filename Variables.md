# 🔧 Understanding Variables in C

A **variable** is a named location in memory used to store a value. In C, before you can use a variable, you must **declare** it — which means telling the compiler what **type of data** it will store.

---

## 📌 What Is a Variable?

- Think of a variable as a **labeled box** that holds a value.
- Each variable has:
  - A **name** (identifier)
  - A **type** (e.g. `int`, `float`, `char`)
  - A **value** stored in memory

---

## 🧪 Example

```c
#include <stdio.h>

int main() {
    int age = 25;
    float height = 1.75;
    char grade = 'A';

    printf("Age: %d\n", age);
    printf("Height: %.2f\n", height);
    printf("Grade: %c\n", grade);

    return 0;
}
```

---
## 📋 Summary Table: Variables

| Concept         | Description                                            |
|-----------------|--------------------------------------------------------|
| Variable        | Named memory location for storing data                |
| Declaration     | Tells the compiler the variable’s type                |
| Initialization  | Giving a variable its first value                     |
| Data Type       | Defines what kind of data the variable holds          |
| Naming Rules    | Use letters, digits, underscores; avoid keywords; case-sensitive |

---
## 🎯 Variable Declaration and Initialization
```c
int number;        // Declaration
number = 10;       // Initialization (assignment)

int age = 20;      // Declaration + Initialization
```

---
## 📃 Rules for Naming Variables
- Must begin with a letter or an underscore _

- Can contain letters, digits, and underscores

- Cannot use C keywords (like int, return, etc.)

- Case-sensitive (Age ≠ age)

❗ Variables are essential building blocks of any program — they let you store, update, and use data while your program runs.
