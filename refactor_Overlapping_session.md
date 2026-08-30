# Merge Overlapping Sessions

## Problem

Given a list of sessions represented by:

```text
[start_time, end_time]
```

merge all sessions that overlap.

Return the final list of non-overlapping sessions.

---

## Example

### Input

```python
sessions = [
    [1, 3],
    [8, 10],
    [2, 6],
    [15, 18]
]
```

Each session represents a start and end time:

```text
[1,3]   → starts at 1, ends at 3
[8,10]  → starts at 8, ends at 10
[2,6]   → starts at 2, ends at 6
[15,18] → starts at 15, ends at 18
```

The sessions:

```text
[1,3]
[2,6]
```

overlap.

Therefore they should become:

```text
[1,6]
```

### Output

```python
[
    [1, 6],
    [8, 10],
    [15, 18]
]
```

---

# Main Idea

The easiest approach is:

```text
SORT
  ↓
Take first interval
  ↓
Compare next interval
  ↓
Overlap?
 /     \
YES     NO
 ↓       ↓
MERGE   APPEND
```

The important trick is to **sort the sessions by their starting time first**.

---

# Step 1 — Sort

Original input:

```text
[1,3] [8,10] [2,6] [15,18]
```

After sorting:

```text
[1,3] [2,6] [8,10] [15,18]
```

Python:

```python
sessions.sort()
```

Now overlapping sessions will usually be next to each other.

---

# Step 2 — Start the Result

Take the first session:

```python
merged = [sessions[0]]
```

Now:

```text
merged = [[1,3]]
```

---

# Step 3 — Compare the Next Session

Current result:

```text
[1,3]
```

Next session:

```text
[2,6]
```

We need to ask:

> Does the current session start before the previous session ends?

The condition is:

```text
current_start <= previous_end
```

Here:

```text
2 <= 3
```

True.

Therefore they overlap.

---

# Step 4 — Merge

We have:

```text
[1,3]
[2,6]
```

The merged session becomes:

```text
[1,6]
```

We keep the existing start `1` and update the end.

Python:

```python
last[1] = max(last[1], current[1])
```

Here:

```text
max(3, 6) = 6
```

So:

```text
[1,3]
   ↓
[1,6]
```

---

# Step 5 — No Overlap

Now compare:

```text
previous = [1,6]
current  = [8,10]
```

Check:

```text
8 <= 6
```

False.

Therefore they do NOT overlap.

Simply add `[8,10]`:

```text
merged = [
    [1,6],
    [8,10]
]
```

---

# Python Solution

```python
def merge_sessions(sessions):

    if not sessions:
        return []

    # Sort sessions by starting time
    sessions.sort()

    # Start with first session
    merged = [sessions[0]]

    # Process remaining sessions
    for current in sessions[1:]:

        # Last session already added
        last = merged[-1]

        # Check for overlap
        if current[0] <= last[1]:

            # Merge by extending the end time
            last[1] = max(
                last[1],
                current[1]
            )

        else:

            # No overlap
            merged.append(current)

    return merged
```

---

# Test

```python
sessions = [
    [1, 3],
    [8, 10],
    [2, 6],
    [15, 18]
]

result = merge_sessions(sessions)

print(result)
```

### Output

```text
[[1, 6], [8, 10], [15, 18]]
```

---

# Dry Run

Start:

```text
sessions:

[1,3] [8,10] [2,6] [15,18]
```

Sort:

```text
[1,3] [2,6] [8,10] [15,18]
```

Initialize:

```text
merged = [[1,3]]
```

---

## Iteration 1

```text
last    = [1,3]
current = [2,6]
```

Check:

```text
current_start <= last_end

2 <= 3
```

Yes.

Merge:

```text
last_end = max(3,6)
         = 6
```

Result:

```text
merged = [[1,6]]
```

---

## Iteration 2

```text
last    = [1,6]
current = [8,10]
```

Check:

```text
8 <= 6
```

No.

Append:

```text
merged = [
    [1,6],
    [8,10]
]
```

---

## Iteration 3

```text
last    = [8,10]
current = [15,18]
```

Check:

```text
15 <= 10
```

No.

Append:

```text
merged = [
    [1,6],
    [8,10],
    [15,18]
]
```

Final answer:

```text
[[1,6], [8,10], [15,18]]
```

---

# Important Formula

This is the most important thing to remember:

```text
current_start <= previous_end
```

means the intervals overlap.

In Python:

```python
if current[0] <= last[1]:
```

Remember:

```text
current[0] → current START

current[1] → current END

last[0] → previous START

last[1] → previous END
```

So:

```python
if current[0] <= last[1]:
```

means:

```text
If CURRENT START
is less than or equal to
PREVIOUS END

→ OVERLAP
```

---

# Why Do We Sort First?

Suppose the input is:

```text
[1,3]
[15,18]
[2,6]
[8,10]
```

Without sorting, `[1,3]` and `[2,6]` are separated.

After sorting:

```text
[1,3]
[2,6]
[8,10]
[15,18]
```

Now overlapping intervals are next to each other.

That allows us to process the sessions in one pass after sorting.

---

# Another Example

Input:

```python
sessions = [
    [1, 5],
    [3, 7],
    [8, 10]
]
```

Sorted:

```text
[1,5]
[3,7]
[8,10]
```

Compare:

```text
[1,5]
[3,7]

3 <= 5
```

Overlap.

Merge:

```text
[1,7]
```

Next:

```text
[1,7]
[8,10]

8 <= 7
```

False.

Final:

```text
[[1,7], [8,10]]
```

---

# Another Important Example — One Interval Inside Another

Suppose:

```text
[1,10]
[2,5]
```

They overlap because:

```text
2 <= 10
```

Now calculate:

```python
max(10, 5)
```

which is:

```text
10
```

So:

```text
[1,10] + [2,5]

        ↓

      [1,10]
```

This is why we use:

```python
last[1] = max(last[1], current[1])
```

instead of simply assigning `current[1]`.

---

# Complexity

Sorting takes:

```text
O(n log n)
```

The loop through the sorted sessions takes:

```text
O(n)
```

Therefore overall:

```text
O(n log n)
```

The sorting step dominates the running time.

---

# Interview Pattern

When you see questions containing:

```text
interval
meeting time
session time
start/end time
overlapping ranges
appointment times
booking times
```

think:

```text
INTERVAL PROBLEM
```

Then ask:

```text
Can I sort by START time?
```

For merging intervals, the pattern is:

```text
SORT BY START
      ↓
Take first interval
      ↓
Compare current START
with previous END
      ↓
current_start <= previous_end ?
      ↓
     YES
      ↓
    MERGE
```

---

# Key Formula

### Detect overlap

```python
current[0] <= last[1]
```

### Merge

```python
last[1] = max(
    last[1],
    current[1]
)
```

Those are the two most important lines in the solution.

---

# Quick Interview Explanation

If an interviewer asks how you would solve the problem:

> First, I would sort the intervals by their starting time. I would add the first interval to my result. Then I would iterate through the remaining intervals and compare each interval's start time with the end time of the last merged interval. If the current start is less than or equal to the previous end, the intervals overlap, so I extend the previous end using `max()`. Otherwise, I append the current interval as a new interval.

---

# Pattern to Remember

```text
MERGE INTERVALS

1. SORT
      ↓
2. TAKE FIRST
      ↓
3. LOOP
      ↓
4. current_start <= previous_end?
      ↓
   YES → MERGE
   NO  → APPEND
```

## Key Takeaway

**Sort first, then compare only with the last merged interval.**

You do not need to compare every interval with every other interval.
