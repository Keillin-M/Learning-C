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

## 🖼️ Pipex Flow: Visual Breakdown

```lua
             +-----------------+          +-----------------+
             |     infile      |          |     outfile     |
             +--------+--------+          +--------+--------+
                      |                            ^
                      v                            |
                +------+-----+             +-------+--------+
                |   cmd1     |  stdout --> |   pipe write   |
                | (child 1)  |             +-------+--------+
                +------+-----+                     |
                      |                            v
         stdin <------+                    +-------+--------+
                                           |   pipe read    |
                                           +-------+--------+
                                                   |
                                                   v
                                           +-------+--------+
                                           |  cmd2 (child2) |
                                           +----------------+

```
---

## 🧠 What's Happening Here:

1. **Create a pipe**

   ```c
   int fd[2];
   pipe(fd);  // fd[0] = read end, fd[1] = write end
   ```

2. **Fork first child (cmd1)**

   * Redirect `infile` to `stdin` using `dup2(infile_fd, 0)`
   * Redirect `pipe` write end to `stdout`: `dup2(fd[1], 1)`
   * Close unused ends: `close(fd[0])`

3. **Fork second child (cmd2)**

   * Redirect `pipe` read end to `stdin`: `dup2(fd[0], 0)`
   * Redirect `outfile` to `stdout`: `dup2(outfile_fd, 1)`
   * Close unused ends: `close(fd[1])`

4. **Parent process**

   * Closes both `fd[0]` and `fd[1]`
   * Waits for both children to finish

---

## 🎯 Data Flow

1. `cmd1` **reads** from the `infile`, **writes** to the pipe.
2. `cmd2` **reads** from the pipe, **writes** to the `outfile`.

---

## 🧪 Example

If you run:

```bash
./pipex infile "grep hello" "wc -l" outfile
```

You’re doing this:

```bash
< infile grep hello | wc -l > outfile
```

* `grep hello` gets its input from `infile`, sends matches to pipe
* `wc -l` reads matches from pipe, counts lines, sends result to `outfile`

