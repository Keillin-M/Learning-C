# 🧠 What Is `readline`?

`readline` is a **function from the GNU Readline library**. It lets your shell read a full line of user input — **with advanced features** like:

* Line editing (using arrow keys, backspace, etc.)
* Command history (use ↑ / ↓ to see previous commands)
* Handling special characters
* Auto-completion (optional)

It’s much more powerful and user-friendly than simpler input methods like `scanf()` or `fgets()`.

---

## 🛠️ What Does `readline` Do Exactly?

When you call `readline("minishell$ ")`, this happens:

1. It **prints** your prompt (`minishell$ `).
2. It **waits** for the user to type something and press Enter.
3. It **returns** the line of text the user typed — as a string you can work with.

---

## ✅ Benefits of Using `readline`

| Feature       | Why It’s Useful                                          |
| ------------- | -------------------------------------------------------- |
| Editing       | User can move cursor, delete, edit text inline.          |
| History       | Keeps a history of commands; user can cycle with ↑ / ↓.  |
| Clean Input   | Automatically handles buffer sizes and memory for you.   |
| Customization | You can later add tab-completion or custom key bindings. |

---

## ⚠️ Things to Know

* `readline` returns `NULL` if the user presses `Ctrl+D` (EOF).
* You must **free the memory** it returns when you're done using the input.
* The readline library isn't part of standard C, so you may need to **link it** with `-lreadline` during compilation.

---

## 🧪 Example Usage (In Words)

Let’s say your shell does this:

> Call `readline("minishell$ ")`
> User types: `echo hello world`
> You get the string: `"echo hello world"`
> You then process and execute it.

---

## 📦 Installing It (For Your Environment)

To use `readline`, you may need to install the library:

* On Debian/Ubuntu: `sudo apt install libreadline-dev`
* On macOS: `brew install readline`

---

## 🔁 Extra: Adding to History

If you want users to use ↑ and ↓ to cycle through commands:

* After getting a line from `readline`, you can store it in history using `add_history(line)`.
