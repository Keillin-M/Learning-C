# 🧠 What Is Quicksort?
Quicksort is a method to sort a list of numbers by dividing it into smaller parts and sorting those parts one at a time. It’s called “quick” because it works faster than many other methods in most cases.

## 🧩 Imagine This:
You have a messy row of cards on a table with numbers on them:

```c
[8, 3, 5, 1, 9, 6]
```
You want to sort them from smallest to biggest.

## 🪄 Step-by-Step Quicksort
**Step 1: Choose a Pivot**

Pick one number in the list to be your pivot. Let’s say we always pick the last number. In our case, the pivot is:
```c
Pivot = 6
```
**Step 2: Rearrange**

Now, look at all the other numbers and move them around so that:
- All the numbers smaller than the pivot (6) go to the left
- All the numbers bigger than the pivot go to the right
So, we rearrange the list like this:
```c
[3, 5, 1]  [6]  [8, 9]
```
Now 6 is in its correct final position in the sorted list!

**Step 3: Repeat on Each Side**

Now take the two smaller parts (the left and right sides) and repeat the same process:

Left part: [3, 5, 1]
- Pivot = 1
- Rearranged: [ ] [1] [3, 5]

Do it again on [3, 5]:
- Pivot = 5
- Rearranged: [3] [5] [ ]

Right part: [8, 9]
- Pivot = 9
- Rearranged: [8] [9] [ ]

## ✅ Final Sorted List
Now put all the pieces together in their sorted positions:
```c
[1, 3, 5, 6, 8, 9]
```
## 🔁 Summary of How It Works
1. Pick a pivot (a number to compare others against)
2. Move smaller numbers to the left, and larger numbers to the right
3. Put the pivot in the middle
4. Repeat for the left and right groups
5. Keep doing that until every part is sorted

## 🧠 Why It’s Fast
Because instead of checking every pair of numbers like bubble sort (which is slow), quicksort splits the problem in half again and again, making it much more efficient.
