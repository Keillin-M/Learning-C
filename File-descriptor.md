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

---

**Incremental section**

* I/O redirection (`dup2`)
* Appending to files
* Connecting pipes using file descriptors

---

### ➕ Extra: File Descriptors in `pipex` and Shell Redirection

In `pipex`, you'll be working **directly** with file descriptors to mimic shell behavior like this:

```bash
< infile cmd1 | cmd2 > outfile
```
---

### 🔄 Redirect Input and Output with `dup2`

You don’t just read and write directly — you often **redirect** input/output to other files or pipes.

```c
dup2(fd, STDOUT_FILENO);  // Redirect output (stdout)
dup2(fd, STDIN_FILENO);   // Redirect input (stdin)
```

This replaces `stdin` or `stdout` with your own file descriptor (like a pipe or opened file).
After this, anything written to `stdout` goes into your file or pipe instead.

---

### 🧪 Example: Replacing STDOUT

```c
int out_fd = open("outfile.txt", O_WRONLY | O_CREAT | O_TRUNC, 0644);
dup2(out_fd, STDOUT_FILENO);  // All printf/write now go to "outfile.txt"
```

---

### ➕ Appending Instead of Overwriting

If you want to **append** to the output file (like in the `here_doc` bonus):

```c
int fd = open("outfile.txt", O_WRONLY | O_CREAT | O_APPEND, 0644);
```

* `O_APPEND`: writes go to the end of the file
* `O_TRUNC`: overwrites the file (default in normal mode)

---

### 🔗 Using Pipes with File Descriptors

When you create a pipe:

```c
int fd[2];
pipe(fd);  // fd[0] = read end, fd[1] = write end
```

To connect one command's output to another’s input:

* `dup2(fd[1], STDOUT_FILENO);` → cmd1 writes to the pipe
* `dup2(fd[0], STDIN_FILENO);` → cmd2 reads from the pipe

---

### 🔒 Always Close Unused File Descriptors

After duplicating with `dup2()`, **close the original**:

```c
close(fd[1]);  // After duplicating write end
close(fd[0]);  // After duplicating read end
```

If you don’t close them, your program may **hang** because pipes wait for EOF (end-of-file), which never comes if descriptors remain open.

---

### 🔁 Summary

| Tool       | Purpose                                        |
| ---------- | ---------------------------------------------- |
| `dup2()`   | Redirect `stdin` or `stdout` to a file or pipe |
| `O_TRUNC`  | Truncate file (clear contents)                 |
| `O_APPEND` | Append to file instead of overwriting          |
| `pipe()`   | Create a unidirectional communication channel  |
| `close()`  | Release unused file descriptors                |

