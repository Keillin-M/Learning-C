# 🛑 What Are `wait()` and `waitpid()`?

Both functions are used by a **parent process** to **wait for one or more child processes to finish** and collect their exit status. This prevents **zombie processes** from piling up.

---

### 1. `wait()`

```c
pid_t wait(int *status);
```

* Suspends the calling process **until any child process terminates**.
* Returns the PID of the terminated child.
* Stores the child's exit status in `status` (if `status` is not `NULL`).

**Simple but only waits for one child at a time, and you don’t control which child.**

---

### 2. `waitpid()`

```c
pid_t waitpid(pid_t pid, int *status, int options);
```

* More flexible than `wait()`.
* Waits for a **specific child process** (`pid`).
* `pid` can be:

  * `> 0`: wait for child with this PID
  * `-1`: wait for any child (same as `wait()`)
  * `0`: wait for any child in the same process group
  * `< -1`: wait for any child in the process group `-pid`
* `options`: can be `0` or flags (like `WNOHANG` to return immediately if no child finished).

---

## 🧪 Example

```c
int status;
pid_t pid = fork();

if (pid == 0) {
    // Child process
    execlp("ls", "ls", NULL);
} else {
    // Parent process waits for this specific child
    waitpid(pid, &status, 0);

    if (WIFEXITED(status)) {
        int exit_code = WEXITSTATUS(status);
        printf("Child exited with code %d\n", exit_code);
    }
}
```

---

## 🔗 Why Are They Important in `pipex`?

* You `fork()` children to run commands (`cmd1`, `cmd2`).
* The parent **must wait** for all children to finish before exiting, to:

  * Avoid zombie processes.
  * Get their exit statuses.
  * Ensure proper synchronization.

---

## 📄 Summary Table

| Function    | Purpose                                 | Key Features                               |
| ----------- | --------------------------------------- | ------------------------------------------ |
| `wait()`    | Waits for **any** child to finish       | Simple, blocks until child exits           |
| `waitpid()` | Waits for a **specific** child or group | Flexible, can be non-blocking or selective |

---

## 🧠 Quick Tips

* Use `waitpid()` in `pipex` to wait for each child by PID.
* Check the child’s exit status with macros like `WIFEXITED(status)` and `WEXITSTATUS(status)`.
* Always wait for children to avoid zombies.

