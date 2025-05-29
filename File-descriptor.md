# 🔧 File Descriptors in C

In C programming, especially on Unix/Linux systems, a **file descriptor** is a low-level integer handle that the operating system uses to manage open files or input/output resources like files, terminals, or network connections.

When you open a file using system calls like `open()`, the OS returns a file descriptor — a small integer — that uniquely identifies that open file for your program. You then use this descriptor with functions like `read()`, `write()`, and `close()` to perform I/O operations.

---

## 📌 How File Descriptors Work

- A file descriptor is simply an **integer number**.
- It points to an entry in the operating system's file table.
- File descriptors 0, 1, and 2 are reserved for **standard input**, **standard output**, and **standard error** respectively.
- When you open new files or devices, the OS assigns the lowest unused file descriptor number.

---

## 🧪 Common System Calls Using File Descriptors

| Function  | Description                           |
|-----------|-------------------------------------|
| `open()`  | Opens a file and returns a descriptor|
| `read()`  | Reads data from a file descriptor    |
| `write()` | Writes data to a file descriptor     |
| `close()` | Closes the file descriptor           |

---

## 📄 Example

```c
#include <fcntl.h>
#include <unistd.h>

int main() {
    int fd = open("file.txt", O_WRONLY | O_CREAT, 0644);
    if (fd == -1) {
        // Handle error
        return 1;
    }

    write(fd, "Hello, world!\n", 14);
    close(fd);

    return 0;
}
```


❗File descriptors provide a direct, efficient way to work with files and I/O in Unix-like systems, but they require careful management to avoid leaks or errors.

---
# 🧠 Analogy: Library Book Checkout
Imagine you want to borrow books from a library:

- When you open a file, the OS gives you a library card number (file descriptor).

- You use this number to read, write, or return the book.

- You always return the card (close the descriptor) when done.

- The library has special cards numbered 0, 1, and 2 reserved for common uses like talking to the librarian (standard input/output/error).

## 📋 Summary Table: File Descriptor Basics

| Concept         | Description                                          |
|-----------------|------------------------------------------------------|
| File Descriptor | Integer ID representing an open file or resource    |
| `open()`        | Opens a file, returns a file descriptor             |
| `read()`        | Reads data using a file descriptor                  |
| `write()`       | Writes data using a file descriptor                 |
| `close()`       | Closes the file descriptor and frees the resource   |
| Standard FDs    | `0` = stdin, `1` = stdout, `2` = stderr              |

## 🛠️ System Call Functions

| System Call | Purpose                      |
|-------------|------------------------------|
| `open()`    | Open or create a file        |
| `read()`    | Read bytes from a descriptor |
| `write()`   | Write bytes to a descriptor  |
| `close()`   | Close and release descriptor |
