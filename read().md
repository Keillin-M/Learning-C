# 📖 `read()` Function in C

In C (especially on Unix/Linux systems), the `read()` function is used for **low-level input**, reading data directly from a file, device, or file descriptor.

It comes from the `<unistd.h>` header and works with **file descriptors**, not `FILE *` streams (used by `fopen`, `fread`, etc.).

---

## 📌 Function Signature

```c
ssize_t read(int fd, void *buffer, size_t count);
```

## 📥 Parameters 

| Parameter | Type         | Description                                            |
|-----------|--------------|--------------------------------------------------------|
| `fd`      | `int`        | File descriptor of the file or resource to read from   |
| `buffer`  | `void *`     | Pointer to a memory block where the read data will go  |
| `count`   | `size_t`     | Maximum number of bytes to read                        |

## Return Value:
- Returns the number of bytes actually read

- Returns 0 if end of file (EOF) is reached

- Returns -1 on error

## 🧪 Example
```c
#include <fcntl.h>
#include <unistd.h>
#include <stdio.h>

int main() {
    char buffer[100];
    int fd = open("example.txt", O_RDONLY);

    if (fd == -1) {
        perror("Error opening file");
        return 1;
    }

    ssize_t bytesRead = read(fd, buffer, sizeof(buffer) - 1);
    if (bytesRead == -1) {
        perror("Error reading file");
        close(fd);
        return 1;
    }

    buffer[bytesRead] = '\0';  // Null-terminate the string
    printf("File contents:\n%s\n", buffer);

    close(fd);
    return 0;
}
```
---

## 🔍 Key Points
- read() is low-level: you manage buffers and bytes manually.

- It doesn’t automatically add '\0' — you must null-terminate strings yourself.

- Used with file descriptors, unlike fread() which is for FILE * streams.

---

## 📋 Summary Table: read()

| Concept         | Description                                       |
|-----------------|---------------------------------------------------|
| Purpose         | Reads raw bytes from a file descriptor            |
| Header          | `<unistd.h>`                                      |
| Parameters      | `fd`, `buffer`, `count`                           |
| Returns         | Number of bytes read, 0 for EOF, -1 for error     |
| Used With       | File descriptors (e.g., from `open()`)            |
| Not Null-Terminated | You must manually add `'\0'` for strings     |

## 🧠 Analogy for Beginners
Imagine reading a book using a slip of paper (file descriptor) to mark where you are:

- You say, “Give me up to 100 words from where my bookmark is.”

- read() gives you that many words (if available), and moves the bookmark.

- If there’s nothing left to read (EOF), it gives you 0.

- If there’s a problem (e.g., the book is missing), it returns -1.


❗ read() is powerful and efficient, but requires careful handling. It’s ideal when performance matters or when you're building low-level systems or tools.
