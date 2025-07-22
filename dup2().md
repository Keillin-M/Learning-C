# 🔁 `dup2()` — Duplicate File Descriptors

### 📌 Purpose:

`dup2(old_fd, new_fd)` duplicates `old_fd` onto `new_fd`.

> It redirects **standard input/output** (or any fd) to another **file or pipe**.

---

### 📚 Syntax:

```c
int dup2(int old_fd, int new_fd);
```

* If `new_fd` is already open, it's closed first.
* After calling `dup2`, **`new_fd` becomes a copy of `old_fd`**.
* Now, any read/write to `new_fd` will go to the same place as `old_fd`.

---

## 🧠 Example 1: Redirect `stdout` to a file

```c
int file_fd = open("output.txt", O_WRONLY | O_CREAT | O_TRUNC, 0644);
dup2(file_fd, STDOUT_FILENO);  // STDOUT now points to output.txt
write(1, "hello\n", 6);         // writes to the file, not the terminal
```

---

## 🧠 Example 2: Redirect `stdin` from a file

```c
int file_fd = open("input.txt", O_RDONLY);
dup2(file_fd, STDIN_FILENO);   // STDIN now points to input.txt
char buffer[100];
read(0, buffer, 100);          // reads from input.txt
```

---

## 🔗 In `pipex`

Let’s say you're running `cmd1` and you want:

* Input from `infile`
* Output sent to the **write end** of a pipe

### ✅ Here's what you do in the child process:

```c
int pipe_fd[2];
pipe(pipe_fd); // pipe_fd[0] = read end, pipe_fd[1] = write end

int infile_fd = open("infile", O_RDONLY);

if (fork() == 0) {
    dup2(infile_fd, STDIN_FILENO);   // stdin <- infile
    dup2(pipe_fd[1], STDOUT_FILENO); // stdout -> pipe
    close(pipe_fd[0]);               // Close unused read end
    close(infile_fd);                // Close original fd after dup2
    execve(cmd1_path, cmd1_args, envp);
}
```

The **second child** (for `cmd2`) would do the reverse:

```c
if (fork() == 0) {
    dup2(pipe_fd[0], STDIN_FILENO);   // stdin <- pipe
    dup2(outfile_fd, STDOUT_FILENO);  // stdout -> outfile
    close(pipe_fd[1]);                // Close unused write end
    execve(cmd2_path, cmd2_args, envp);
}
```

---

## ⚠️ Must-Do After `dup2()`

* ✅ **Close the original file descriptors** (`fd`, `pipe_fd[x]`) after duplication.
* ❌ Don’t use them after `dup2` — they’re no longer needed.

---

## 🧪 TL;DR

| `dup2()` usage            | What it does                        |
| ------------------------- | ----------------------------------- |
| `dup2(fd, 0)`             | Redirects `stdin` to read from `fd` |
| `dup2(fd, 1)`             | Redirects `stdout` to write to `fd` |
| Used before `execve()`    | To set up command input/output      |
| Closes `new_fd` if needed | Prevents fd leaks                   |

