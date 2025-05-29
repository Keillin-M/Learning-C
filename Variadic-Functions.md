# 🔧 Understanding Variadic Functions in C

In C, **variadic functions** are functions that accept a **variable number of arguments**. This allows you to create flexible functions where the exact number of parameters is not known at compile time.

---

## 📌 What Are Variadic Functions?

Variadic functions use an ellipsis (`...`) in their declaration to indicate they can take additional arguments beyond those explicitly named.

Example from the standard library:
```c
int printf(const char *format, ...);
```
---

🛠 How to Declare and Use Variadic Functions
To handle the variable arguments, C provides macros in the <stdarg.h> header:

va_list: a type to hold information about variable arguments.

va_start: initialize the va_list variable.

va_arg: retrieve the next argument in the list.

va_end: clean up.

---

🧪 Example: Custom sum Function
```c
#include <stdio.h>
#include <stdarg.h>

int sum(int count, ...) {
    va_list args;
    va_start(args, count);

    int total = 0;
    for (int i = 0; i < count; i++) {
        total += va_arg(args, int);  // Retrieve each argument as int
    }

    va_end(args);
    return total;
}

int main() {
    printf("Sum: %d\n", sum(3, 10, 20, 30));       // Output: 60
    printf("Sum: %d\n", sum(5, 1, 2, 3, 4, 5));    // Output: 15
    return 0;
}
```
---

## 🧠 Summary Table: Static Variables

| Use Case                   | Description                                      |
|----------------------------|--------------------------------------------------|
| Inside a function          | Retains value across calls                       |
| Global variable with `static` | Limits visibility to the current file        |
| Function with `static`     | Makes the function only visible in that file     |

---
⚠️ Important Notes
The function must know how many arguments or their types in some way (e.g., by passing a count as in the example).

No type checking is enforced by the compiler for variadic arguments.

Use with care to avoid undefined behavior.

---
## ✅ Summary Table: Variadic Functions

| Feature              | Description                                 |
|----------------------|---------------------------------------------|
| `...` in function    | Declares a variable number of arguments     |
| `<stdarg.h>` macros  | Used to access those arguments safely       |
| Common use cases     | `printf`, loggers, mathematical helpers    |

