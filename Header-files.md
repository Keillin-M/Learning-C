# 🔧 Understanding Header Files in C

In C programming, a **header file** is a file with a `.h` extension that contains **declarations** for functions, constants, macros, and sometimes data types. Header files help organize code and enable **code reuse** across multiple `.c` files.

---

## 📌 What Is a Header File?

- A header file is like a **contract** or **interface** — it tells the compiler **what exists** but not necessarily **how it's implemented**.
- You include a header file using `#include`.
- Common examples: `stdio.h`, `stdlib.h`, `string.h`.

---

## 🔧 What Goes Inside a Header File?

- Function declarations (prototypes)
- Constant definitions (`#define`)
- Macros
- Struct or type definitions
- External variable declarations
- Include guards (to prevent multiple inclusions)

---

## 🧪 Example: Using a Custom Header File

### 📁 `mathutils.h` (header file)

```c
#ifndef MATHUTILS_H
#define MATHUTILS_H

int add(int a, int b);
int multiply(int a, int b);

#endif
```

---
## 📁 mathutils.c (implementation file)
```c
#include "mathutils.h"

int add(int a, int b) {
    return a + b;
}

int multiply(int a, int b) {
    return a * b;
}
```
## 📁 main.c (main program)
```c
#include <stdio.h>
#include "mathutils.h"

int main() {
    printf("Add: %d\n", add(3, 4));
    printf("Multiply: %d\n", multiply(3, 4));
    return 0;
}
```

---
## 🔐 What Are Include Guards?
Include guards prevent the same header file from being included multiple times, which can cause redefinition errors.
```c
#ifndef HEADER_NAME
#define HEADER_NAME
// declarations
#endif
```

---
## 📋 Summary Table: Header Files

| Concept           | Description                                             |
|-------------------|---------------------------------------------------------|
| Header File (`.h`) | Declares functions, macros, constants, and types       |
| `#include`         | Tells the compiler to use the header file's contents   |
| Function Prototype | Declares a function’s name, return type, and parameters|
| Include Guard      | Prevents the header from being included multiple times |
| Separation of Code | Keeps declarations (header) separate from implementation |


❗Header files promote code reuse, separation of concerns, and cleaner, more maintainable code.
