# 🧬 What is `fork()`?

`fork()` is a system call in C (and Unix-like systems) that **creates a new process** by **duplicating the current one**.

> ✅ You end up with **two processes**:
>
> * The **parent** (original)
> * The **child** (new)

They both **continue running the same code**, but you can tell them apart by the **return value of `fork()`**.

---

## 🔁 Basic Behavior

```c
pid_t pid = fork();

if (pid == -1) {
    // Error: fork failed
} else if (pid == 0) {
    // This is the child process
} else {
    // This is the parent process
}
```

* `pid > 0`: you're in the **parent**, and `pid` is the child’s PID
* `pid == 0`: you're in the **child**
* `pid == -1`: fork failed (e.g., too many processes)

---

## 🧠 Why is `fork()` Useful?

Because it allows you to:

* Run **multiple processes in parallel**
* Have each process do **different tasks**
* Use one process (the child) to **execute a new program** (with `execve()`)

In `pipex`, you’ll use `fork()` to:

* Create a child that runs `cmd1`
* Create a second child that runs `cmd2`
* Connect them via a `pipe`

---

## 🔗 Example in Context (pipex-style)

```c
int fd[2];
pipe(fd);

pid_t pid = fork();

if (pid == 0) {
    // Child process: run cmd1
    dup2(infile_fd, STDIN_FILENO);
    dup2(fd[1], STDOUT_FILENO);
    close(fd[0]);
    execve(cmd1_path, cmd1_args, envp);
} else {
    // Parent process: fork another child for cmd2
    pid_t pid2 = fork();
    if (pid2 == 0) {
        // Second child: run cmd2
        dup2(fd[0], STDIN_FILENO);
        dup2(outfile_fd, STDOUT_FILENO);
        close(fd[1]);
        execve(cmd2_path, cmd2_args, envp);
    }
    // Parent closes both ends and waits
    close(fd[0]);
    close(fd[1]);
    waitpid(pid, NULL, 0);
    waitpid(pid2, NULL, 0);
}
```

---

## 🎯 Summary

| Term       | Meaning                           |
| ---------- | --------------------------------- |
| `fork()`   | Duplicates current process        |
| Parent     | Original process after `fork()`   |
| Child      | New process that runs in parallel |
| Return 0   | In child process                  |
| Return > 0 | In parent process (child PID)     |
| Return -1  | Error: fork failed                |

---

## 🧾 What Is a PID?

**PID** stands for **Process ID** — it’s a **unique number** the operating system assigns to **each running process**.

Think of it like a **social security number** for processes. Every time a new process is created (e.g., using `fork()`), it gets a new PID.

---

## 📍 Why Is PID Important?

When you call `fork()`, it creates a **child process**, and that child gets its own PID.

The return value of `fork()` helps you know **which process you're in**:

```c
pid_t pid = fork();

if (pid == 0) {
    // 👶 This is the child process
    printf("I'm the child. My PID is %d\n", getpid());
} else if (pid > 0) {
    // 👨 This is the parent process
    printf("I'm the parent. My child's PID is %d\n", pid);
} else {
    // ❌ Fork failed
}
```

---

## 🧠 Common PID-related Functions

| Function            | Description                                           |
| ------------------- | ----------------------------------------------------- |
| `getpid()`          | Returns the PID of **this** process                   |
| `getppid()`         | Returns the PID of **parent**                         |
| `fork()`            | Returns the **child PID** to parent, and `0` to child |
| `waitpid(pid, ...)` | Waits for a specific process to finish                |

---

## 🧪 Example Output

```bash
I'm the parent. My child's PID is 4567
I'm the child. My PID is 4567
```

---

## 🔄 In `pipex`, how is PID used?

You use `pid` to:

* Know when you're in the child (so you can run `execve`)
* Let the parent `waitpid(pid)` for each child, so it doesn't create zombie processes
* Track or debug which child is doing what

---

## 🔑 Summary

* **PID** = Process ID (unique to every process)
* Used to control, track, or wait for processes
* Critical when working with `fork()` and `execve()`
