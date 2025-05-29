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

---
# 🧠 Common Data Types in C

C provides a range of built-in data types that let you store different kinds of information, such as numbers, characters, or memory addresses. Each type defines **what kind of data** a variable holds and **how much memory** it consumes.

---

## 📦 Basic Data Types

| Type        | Description                                | Example Value     |
|-------------|--------------------------------------------|-------------------|
| `int`       | Integer numbers                            | `42`, `-1`        |
| `float`     | Decimal numbers (single precision)         | `3.14`, `-0.5`    |
| `double`    | Decimal numbers (double precision)         | `2.71828`         |
| `char`      | Single character                           | `'A'`, `'z'`      |
| `void`      | No value / no type (used in functions)     | *(no value)*      |

---

## 📦 Extended Integer Types

| Type             | Description                              | Notes                                |
|------------------|------------------------------------------|--------------------------------------|
| `long int`       | Larger integer than `int`                | More range than `int`                |
| `unsigned int`   | Only positive integers                   | No negative values                   |
| `short int`      | Smaller integer range                    | Less memory used                     |
| `unsigned long`  | Large positive integer                   | No negatives, wide range             |

---

## 🧵 Character & String Types

| Type       | Description                                      | Example               |
|------------|--------------------------------------------------|------------------------|
| `char`     | Single ASCII character                           | `'x'`, `'9'`           |
| `char *`   | Pointer to a string (character array)            | `"Hello"`, `"World"`   |

---

## 📐 Special Type

| Type      | Description                                      | Common Use                      |
|-----------|--------------------------------------------------|----------------------------------|
| `size_t`  | Unsigned integer type used for sizes or indexes  | Loops, `sizeof()`, arrays        |

---

## 📋 Summary Table: Common C Data Types

| Category           | Type              | Description                                      |
|--------------------|-------------------|--------------------------------------------------|
| Integer            | `int`             | Basic whole number                               |
| Integer (large)    | `long int`        | Larger integer                                   |
| Integer (small)    | `short int`       | Smaller integer                                  |
| Unsigned           | `unsigned int`    | No negatives, larger positive range              |
| Decimal            | `float`, `double` | Real numbers (single/double precision)           |
| Character          | `char`            | A single character                               |
| String             | `char *`          | Points to a sequence of characters (string)      |
| Void               | `void`            | Represents no value (e.g., for functions)        |
| Size               | `size_t`          | Used for size/index, especially with arrays      |


❗ Choosing the right data type ensures your program uses memory efficiently and handles data safely.

