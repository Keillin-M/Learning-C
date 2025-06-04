# 🔧 Understanding Linked Lists in C

A **linked list** is a fundamental data structure consisting of nodes where each node contains data and a pointer to the next node in the sequence. Unlike arrays, linked lists provide dynamic memory allocation and ease of insertion/deletion.

---

## 📌 What is a Linked List?

- A linked list is made up of **nodes**.
- Each node contains:
  - **Data** (can be any type)
  - A **pointer** to the next node (`next`).
- The list starts with a pointer called `head` which points to the first node.
- The last node points to `NULL`, indicating the end of the list.

---

## 🧪 Basic Structure of a Node

```c
struct Node {
    int data;
    struct Node *next;
};
```
---

## 🧪 Example: Creating and Traversing a Linked List
```c
#include <stdio.h>
#include <stdlib.h>

struct Node {
    int data;
    struct Node *next;
};

int main() {
    // Create nodes
    struct Node *head = NULL;
    struct Node *second = NULL;
    struct Node *third = NULL;

    // Allocate memory
    head = (struct Node*)malloc(sizeof(struct Node));
    second = (struct Node*)malloc(sizeof(struct Node));
    third = (struct Node*)malloc(sizeof(struct Node));

    // Assign data and link nodes
    head->data = 1;
    head->next = second;

    second->data = 2;
    second->next = third;

    third->data = 3;
    third->next = NULL;

    // Traverse and print the list
    struct Node *ptr = head;
    while (ptr != NULL) {
        printf("%d -> ", ptr->data);
        ptr = ptr->next;
    }
    printf("NULL\n");

    // Free memory (good practice)
    free(third);
    free(second);
    free(head);

    return 0;
}
```
---

## 🔑 Key Operations on Linked Lists

| Operation          | Description                          |
|--------------------|------------------------------------|
| Traversal          | Visiting each node sequentially     |
| Insertion          | Adding a node at beginning, middle, or end |
| Deletion           | Removing a node                    |
| Searching          | Finding a node with a given value  |

---

## 📘 Summary Table

| Term           | Description                            |
|----------------|--------------------------------------|
| Node           | Element containing data + next pointer |
| Head           | Pointer to the first node             |
| NULL           | Indicates end of the list             |
| Dynamic Memory | Allocated at runtime with `malloc`   |


❗Linked lists offer flexibility with dynamic size but require careful memory management to avoid leaks or dangling pointers.

---
# 📦 Understanding Linked Lists with Boxes

Imagine you have a **line of boxes** arranged one after another.

- Each **box** contains two things:
  1. Some **stuff inside** (this is the data).
  2. An **arrow pointing** to the next box in the line.

The first box is called the **head** — it’s where you start looking.

You keep following the arrows from box to box, one after the other, until you reach a box that has **no arrow** — this means it’s the last box.

---

## Why Use Boxes Like This?

- You can **add a new box anywhere** in the line easily by changing just a few arrows.
- You can **remove a box** just by skipping its arrow.
- The boxes **don’t have to be all together** on a shelf; they can be spread out, but the arrows keep them connected.

---

## Summary in Box Terms

| Term           | What it Means                          |
|----------------|--------------------------------------|
| Box (Node)     | A container holding stuff + an arrow  |
| Arrow (Pointer)| Shows where the next box is           |
| Head           | The first box you start from          |
| No arrow (NULL)| The last box with nowhere to point    |

---

❗Linked lists are like a chain of boxes connected by arrows, letting you organize and rearrange your stuff flexibly!

---

# 🏷️ Using Linked Lists to Track Multiple Files (File Descriptors)

When you manage multiple files (or file descriptors, fd) in a function like get_next_line, you need to keep track of leftover data separately for each file.

Think of each node in your linked list as a box labeled with the file’s name:

- The label (fd) is like the box’s name or ID — it tells you which file this box belongs to.

- The content of the box (stash) is the leftover data for that particular file.

- The arrow (next) points to the next box in the chain.

## How this works practically:

- When you want to read from a file, you look through the line of boxes for the one whose label (node->fd) matches the file’s descriptor.

- If you find the box, you use the leftover data inside (node->stash).

- If not, you create a new box, label it with that file descriptor, and put it at the end or beginning of the chain.

- This way, each file descriptor has its own dedicated box, keeping its data safe and separate.

## Accessing Data Inside a Node

- Use node->fd to get the file descriptor label (the box’s name).

- Use node->stash (or node->content) to access the leftover data inside the box.

- Use node->next to move to the next box in the linked list.

### Why is the label (fd) important?

Because it helps you find the right box for the file you want to read from. You check each box’s label until you find the match.

| Term              | What It Represents                     |
|-------------------|--------------------------------------|
| `fd`              | The box’s **name or ID** (file number) |
| `node->stash`     | The **content** inside the box (leftover data) |
| `node->next`      | The **arrow** to the next box          |
| Searching `fd`    | Looking for the box with that label    |

❗ This method lets your function remember leftover data for multiple files simultaneously using just one static variable — a box holding a chain of labeled boxes — making your program neat and efficient!
