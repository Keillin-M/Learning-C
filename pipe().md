# 🔗 What Is a `pipe()`?

A **pipe** is a **unidirectional communication channel** between two processes.
It allows one process to **send data** to another — just like connecting a hose from one process’s output to another’s input.

In simple terms:

* You write data on one end.
* You read that data from the other end.

Think of it like:

```
Process A --> pipe --> Process B
```

---

## 🛠️ How `pipe()` Works in C

In C, a pipe is created using the `pipe()` system call:

```c
int fd[2];
pipe(fd);
```

Now you have:

* `fd[0]` — the **read end** of the pipe
* `fd[1]` — the **write end** of the pipe

### 📤 Writing:

```c
write(fd[1], "hello", 5);
```

### 📥 Reading:

```c
char buffer[6];
read(fd[0], buffer, 5);
buffer[5] = '\0';
printf("%s\n", buffer); // prints "hello"
```

---

## 🧠 In Context of `pipex`

Let’s say you’re trying to mimic this shell command:

```bash
cat file.txt | grep hello
```

You want:

* `cat file.txt` to **write** its output to a pipe
* `grep hello` to **read** from that pipe as its input

In `pipex`, this is done by:

1. Creating a pipe.
2. Forking two child processes.
3. First child (`cmd1`) redirects its **stdout** to `fd[1]` (write end).
4. Second child (`cmd2`) redirects its **stdin** to `fd[0]` (read end).
5. Parent closes both ends after forking.

### 📊 Data Flow:

```
[ infile ] → [ cmd1 ] → [ pipe ] → [ cmd2 ] → [ outfile ]
```

---

## 🚨 Important Notes

* **Always close unused ends**:

  * After a `fork()`, each child must close the pipe end it doesn’t use.
* Pipes are **unidirectional** — if you need two-way communication, use two pipes.
* Pipes only pass data — they don't execute code.

---

## 🧪 TL;DR

* `pipe()` gives you two file descriptors: one for reading, one for writing.
* It connects the output of one process to the input of another.
* It’s essential for replicating the behavior of shell pipelines (`|`).

---

```lua
             +-----------------+          +-----------------+
             |     infile      |          |     outfile     |
             +--------+--------+          +--------+--------+
                      |                            ^
                      v                            |
                +-----+-----+              +-------+--------+
                |   cmd1     |  stdout --> |   pipe write   |
                | (child 1)  |             +-------+--------+
                +-----+-----+                     |
                      |                           v
         stdin <-------+                   +-------+--------+
                                          |   pipe read     |
                                          +-------+--------+
                                                  |
                                                  v
                                          +-------+--------+
                                          |   cmd2 (child2) |
                                          +-----------------+

```
