# 🔍 What is `access()`?

`access()` is a system call that checks if a **file or executable exists**, and whether your program has permission to **read**, **write**, or **execute** it.

---

## 📚 Function Prototype

```c
int access(const char *pathname, int mode);
```

### Parameters:

* `pathname`: full path to the file or program (e.g., `/bin/ls`)
* `mode`: what permission you want to check — see below 👇

---

## 🔑 Common Modes:

| Macro  | Checks if…                    |
| ------ | ----------------------------- |
| `F_OK` | The file **exists**           |
| `R_OK` | You can **read** the file     |
| `W_OK` | You can **write** to the file |
| `X_OK` | You can **execute** the file  |

---

## 🧪 Example

```c
if (access("/bin/ls", X_OK) == 0) {
    // ✅ It's executable
} else {
    // ❌ Not found or not executable
}
```

---

## 🔗 Why is it useful in `pipex`?

Because users may give commands like `"ls"` or `"grep"`, and you need to:

1. Search for the **full path** to the binary (e.g., `/bin/ls`, `/usr/bin/grep`)
2. Use `access()` to check if that path:

   * Exists
   * Is executable

So if you're looping over directories in `$PATH`, you might do:

```c
char *paths[] = {"/bin", "/usr/bin", NULL};
char *cmd = "ls";
char fullpath[1024];

for (int i = 0; paths[i]; i++) {
    snprintf(fullpath, sizeof(fullpath), "%s/%s", paths[i], cmd);
    if (access(fullpath, X_OK) == 0) {
        execve(fullpath, args, envp);
    }
}
```

---

## 🛠️ Summary

| Function       | What it does                                                          |
| -------------- | --------------------------------------------------------------------- |
| `access()`     | Checks if a file or program exists and is usable                      |
| Use in `pipex` | To confirm if a command is executable before calling `execve()`       |
| Return value   | `0` = success, `-1` = error (file doesn’t exist or permission denied) |

---

### ✅ Bonus Tip: Always check `access()` **before** calling `execve()`, so you can show a proper error message if the command doesn’t exist.
