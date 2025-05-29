# 🔧 Understanding Structs in C

In C, a **struct** (short for structure) is a way to group different variables under one name. These variables can be of different types and represent a single logical entity.

---

## 📌 What is a Struct?

- A struct is like a **custom data type** that holds several pieces of related data together.
- Each piece of data inside a struct is called a **member** or **field**.
- You can use structs to represent complex data like a person, a point in 2D space, or a book.

---

## 🧪 Basic Example of a Struct

```c
#include <stdio.h>

struct Point {
    int x;
    int y;
};

int main() {
    struct Point p1;
    p1.x = 10;
    p1.y = 20;

    printf("Point p1 is at (%d, %d)\n", p1.x, p1.y);
    return 0;
}
```

---

## 📍 Explanation
struct Point defines a new type with two members: x and y.

p1 is a variable of type struct Point.

We assign values to p1.x and p1.y and then print them.

---

## 📋 Summary Table: Structs

| Concept               | Description                                         |
|-----------------------|-----------------------------------------------------|
| `struct`              | A user-defined data type that groups multiple variables |
| Members               | Variables of different types grouped inside a struct  |
| Declaration           | Defines the structure of the grouped data             |
| Variable of struct    | An instance that holds actual values for the members    |
| Access                | Use the dot operator `.` to access members             |


❗ Structs help organize related data neatly and make programs easier to understand and maintain.

---

# 📦 Struct Analogy: Grouping Items in a Box
Imagine a box that holds different items together.

Each item in the box is a member (like x and y).

The box itself is the struct — it groups all these items in one place.

For example, a person’s profile box might contain a name tag, an age card, and an address note — all different types of information stored together.

## 📋 Summary Table: Structs

| Concept               | Explanation                          |
|-----------------------|------------------------------------|
| Struct                | A box that groups related items     |
| Members               | The individual items inside the box |
| Variable of Struct Type| A specific box holding actual items |
