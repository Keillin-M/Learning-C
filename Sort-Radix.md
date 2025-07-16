# 🧠 What Is Radix Sort?

Radix sort is a way to sort numbers by looking at their digits (or bits) one at a time, starting from the least important digit to the most important. Instead of comparing numbers directly, it groups numbers based on each digit repeatedly until the whole list is sorted.

---

## 🧩 Imagine This:

You have a messy stack of cards with numbers:

```
[5, 2, 4, 1, 3]
```

You want to sort them from smallest to biggest, but instead of comparing numbers, you look at their binary digits (bits) from right to left.

---

## 🪄 Step-by-Step Radix Sort (using bits)

We will sort by bits starting from the least significant bit (rightmost):

---

**Step 1: Look at the rightmost bit (bit 0)**

* Group cards by whether their rightmost bit is 0 or 1.
* Cards with bit 0 = 0 go to one group (call it B).
* Cards with bit 0 = 1 stay in A or get rotated around.
* After processing all cards, put group B back on top of A.

Example:
Numbers & bit 0:

* 5 (101) → bit0=1
* 2 (010) → bit0=0
* 4 (100) → bit0=0
* 1 (001) → bit0=1
* 3 (011) → bit0=1

Group 0 bits in B: `[2, 4]`
Group 1 bits remain in A: `[5, 1, 3]`

After putting B back on top of A:

```
[2, 4, 5, 1, 3]
```

---

**Step 2: Look at the next bit (bit 1)**

Repeat grouping by bit 1:
Numbers & bit 1:

* 2 (010) → bit1=1
* 4 (100) → bit1=0
* 5 (101) → bit1=0
* 1 (001) → bit1=0
* 3 (011) → bit1=1

Group 0 bits in B: `[4, 5, 1]`
Group 1 bits remain in A: `[2, 3]`

Put B back on top of A:

```
[4, 5, 1, 2, 3]
```

---

**Step 3: Look at the next bit (bit 2, most significant)**

Group by bit 2:

* 4 (100) → bit2=1
* 5 (101) → bit2=1
* 1 (001) → bit2=0
* 2 (010) → bit2=0
* 3 (011) → bit2=0

Group 0 bits in B: `[1, 2, 3]`
Group 1 bits remain in A: `[4, 5]`

Put B back on top of A:

```
[1, 2, 3, 4, 5]
```

---

## ✅ Final Sorted List

Now your stack is sorted!

---

## 🔁 Summary of How Radix Sort Works

1. Look at the least important digit (or bit).
2. Group numbers by whether that digit is 0 or 1.
3. Push the group with 0 digit to a helper stack.
4. After all numbers are grouped, push them back to the original stack.
5. Move to the next more important digit.
6. Repeat until all digits are processed.
7. The numbers end up sorted from smallest to biggest.

---

## 🧠 Why It Works Well

Instead of comparing each pair of numbers directly (which can be slow), radix sort sorts numbers by their digits, grouping them step by step. This way, sorting big lists of numbers can be done efficiently, especially if numbers are not too large.
