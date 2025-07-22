# ⚙️ What is `execve()`?

`execve()` is a **system call** in C that **replaces the current process image** with a **new program**.

> In simple terms:
> It **stops your current code**, and **starts running a completely new program**, like `/bin/ls`, inside the **same process**.

---

## 📚 Function Signature

```c
int execve(const char *pathname, char *const argv[], char *const envp[]);
```

### Parameters:

* `pathname`: the **full path** to the executable (e.g., `/bin/ls`)
* `argv[]`: array of arguments (e.g., `["ls", "-l", NULL]`)
* `envp[]`: environment variables (`envp` from `main()`)

### Return:

* Returns **only on failure** (returns `-1`)
* On success, your current program is completely replaced — it doesn’t "return"

---

## 📌 Why Use `execve()`?

In `pipex`, you're mimicking shell behavior:

```bash
cat file.txt | grep hello
```

To do that, each command (`cat`, `grep`, etc.) must be run as a separate program. `execve()` is the low-level tool that actually **runs external commands** like `ls`, `grep`, `wc`, etc.

When you do:

```c
execve("/bin/ls", args, envp);
```

The current process (typically a **child** created by `fork()`) is replaced by `ls`, and it starts executing immediately.

---

## 👣 How It Fits in `pipex`

1. **You call `fork()`** to create a child process.
2. In the child:

   * Set up input/output with `dup2()`
   * Prepare command path and arguments
   * Call `execve(path, args, envp)`
3. If `execve()` succeeds, it replaces the child’s code with the new program.
4. If it fails (e.g., command not found), you must handle the error and exit.

---

## 🧠 Example

```c
char *args[] = {"ls", "-l", NULL};
execve("/bin/ls", args, envp);
```

* This runs `ls -l`
* If successful, your process becomes `ls`
* If not, `execve()` returns `-1`

---

## 💡 Key Points

| Concept                   | Explanation                                       |
| ------------------------- | ------------------------------------------------- |
| `execve()`                | Executes a new program inside the current process |
| Used after `fork()`       | So you don’t replace the main/parent process      |
| Doesn't return on success | Your code ends here; the new program takes over   |
| Needs full path           | You must find `/bin/ls`, `/usr/bin/wc`, etc.      |

---

## 🛠️ TL;DR

* `execve()` **runs external commands** in Unix.
* It’s used in `pipex` to execute `cmd1` and `cmd2`.
* Called from **child processes** after setting up I/O.
* You must **build the path, args, and envp** manually.
