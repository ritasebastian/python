# First Unique Character in a String

## Problem

Given a string, find the **first character that appears only once** and return its **index/position**.

If every character appears more than once, return:

```text id="cr92pn"
-1
```

---

# Example 1

### Input

```python id="hlqk6q"
s = "leetcode"
```

Characters and positions:

```text id="b6clfj"
Index:      0 1 2 3 4 5 6 7
Character:  l e e t c o d e
```

Count each character:

```text id="duxt14"
l → 1
e → 3
t → 1
c → 1
o → 1
d → 1
```

Start checking from the beginning:

```text id="sw2b5m"
l → count = 1 ✅
```

`l` is the first unique character.

### Output

```python id="c7tqkg"
0
```

---

# Example 2

### Input

```python id="bfclz8"
s = "loveleetcode"
```

Positions:

```text id="4pqz5u"
Index:      0 1 2 3 4 5 6 7 8 9 10 11
Character:  l o v e l e e t c o  d  e
```

Character counts:

```text id="lnyig5"
l → 2
o → 2
v → 1
e → 4
t → 1
c → 1
d → 1
```

Now check from left to right:

```text id="99t93j"
Index 0 → l → count 2 ❌
Index 1 → o → count 2 ❌
Index 2 → v → count 1 ✅
```

Therefore:

### Output

```python id="ihxtm1"
2
```

---

# Example 3

### Input

```python id="zshjqk"
s = "aabb"
```

Counts:

```text id="4z6gk5"
a → 2
b → 2
```

There is no character with count `1`.

### Output

```python id="g7zfzf"
-1
```

---

# Main Idea

This problem has two steps:

```text id="mn08wv"
STEP 1
COUNT every character
      ↓
STEP 2
Find the FIRST character
whose count == 1
```

This is a very useful interview pattern:

```text id="dysqfv"
COUNT
  ↓
STORE IN DICTIONARY
  ↓
SCAN AGAIN
  ↓
FIRST COUNT == 1
  ↓
RETURN INDEX
```

---

# Python Solution

```python id="yjy34m"
def first_uniq_char(s):

    char_count = {}

    # Step 1:
    # Count each character
    for char in s:

        if char not in char_count:
            char_count[char] = 1
        else:
            char_count[char] += 1

    # Step 2:
    # Find first character with count 1
    for index, char in enumerate(s):

        if char_count[char] == 1:
            return index

    # No unique character
    return -1
```

---

# Test

```python id="pyd9c6"
print(first_uniq_char("leetcode"))
print(first_uniq_char("loveleetcode"))
print(first_uniq_char("aabb"))
```

### Output

```text id="39brcs"
0
2
-1
```

---

# How the Dictionary Works

Suppose:

```python id="y0m7tc"
s = "loveleetcode"
```

We start with:

```python id="d7yr1u"
char_count = {}
```

Read `l`:

```text id="n10yd7"
{"l": 1}
```

Read `o`:

```text id="ksk0fq"
{"l": 1, "o": 1}
```

Read `v`:

```text id="9ptohd"
{"l": 1, "o": 1, "v": 1}
```

Read `e`:

```text id="aycxhu"
{"l": 1, "o": 1, "v": 1, "e": 1}
```

Later another `l` appears:

```text id="a7zhh7"
{"l": 2, "o": 1, "v": 1, "e": 1}
```

After processing the entire string:

```text id="dwmsq8"
{
    "l": 2,
    "o": 2,
    "v": 1,
    "e": 4,
    "t": 1,
    "c": 1,
    "d": 1
}
```

The dictionary tells us:

```text id="z87u0c"
character → frequency
```

---

# Why Do We Need Two Loops?

This is important.

The first loop answers:

> How many times does each character occur?

```python id="h6m8kd"
for char in s:
```

After this loop, we know the complete frequency of every character.

The second loop answers:

> Which character with count `1` appears FIRST?

```python id="15ws39"
for index, char in enumerate(s):
```

So:

```text id="gnfztc"
FIRST LOOP
   ↓
COUNT


SECOND LOOP
   ↓
FIND FIRST UNIQUE POSITION
```

---

# Why Use `enumerate()`?

We need to return the **index**, not the character.

If we write:

```python id="i2g23u"
for char in s:
```

we only get:

```text id="b86ogp"
l
o
v
e
...
```

But:

```python id="x6ckh5"
for index, char in enumerate(s):
```

gives both:

```text id="0x3o5m"
index     character

0         l
1         o
2         v
3         e
...
```

Therefore we can return:

```python id="6ch8c8"
return index
```

---

# Easier Dictionary Syntax

Python provides `.get()`.

Instead of:

```python id="idq6um"
if char not in char_count:
    char_count[char] = 1
else:
    char_count[char] += 1
```

we can write:

```python id="5axymd"
char_count[char] = char_count.get(char, 0) + 1
```

So the shorter solution is:

```python id="ohff8x"
def first_uniq_char(s):

    char_count = {}

    for char in s:
        char_count[char] = char_count.get(char, 0) + 1

    for index, char in enumerate(s):
        if char_count[char] == 1:
            return index

    return -1
```

---

# Understanding `.get()`

This line:

```python id="76ewsh"
char_count.get(char, 0)
```

means:

> Give me the current count for `char`. If the character doesn't exist yet, give me `0`.

For the first `a`:

```text id="fakzmv"
current count = 0

0 + 1 = 1
```

Dictionary:

```text id="wybaxw"
{"a": 1}
```

Second `a`:

```text id="ejmslq"
current count = 1

1 + 1 = 2
```

Dictionary:

```text id="t0jks6"
{"a": 2}
```

So the formula:

```python id="h53hfm"
char_count[char] = char_count.get(char, 0) + 1
```

means:

```text id="v06f7i"
OLD COUNT + 1
```

---

# Using `Counter`

Python also provides `Counter`.

```python id="81xpcn"
from collections import Counter


def first_uniq_char(s):

    counts = Counter(s)

    for index, char in enumerate(s):

        if counts[char] == 1:
            return index

    return -1
```

`Counter` automatically builds the frequency dictionary.

For:

```python id="h3t6c1"
Counter("aabbc")
```

you can think of it as:

```text id="ldn8ji"
{
    "a": 2,
    "b": 2,
    "c": 1
}
```

For interviews, it is useful to understand the normal dictionary solution first.

---

# Complexity

Suppose the string contains `n` characters.

First loop:

```text id="4o0skq"
O(n)
```

Second loop:

```text id="tbpq19"
O(n)
```

Total:

```text id="cm9e1r"
O(n) + O(n)
```

which simplifies to:

```text id="f3ccm7"
O(n)
```

We do **not** say:

```text id="3jfj2g"
O(n²)
```

because the loops are separate, not nested.

This:

```python id="60jku5"
for char in s:
    ...

for char in s:
    ...
```

is:

```text id="a8p8o4"
O(n) + O(n)
= O(2n)
= O(n)
```

But this:

```python id="j0cl4w"
for char1 in s:
    for char2 in s:
        ...
```

would be:

```text id="m9rb5i"
O(n²)
```

---

# Original Brute-Force Approach

A beginner might write:

```python id="s6tq45"
def first_uniq_char(s):

    for i in range(len(s)):

        count = 0

        for j in range(len(s)):

            if s[i] == s[j]:
                count += 1

        if count == 1:
            return i

    return -1
```

This works, but it compares every character against every other character.

Complexity:

```text id="ftf2ig"
O(n²)
```

The dictionary solution improves this to:

```text id="q6zgob"
O(n)
```

---

# Interview Pattern

When you see questions like:

```text id="4rzq3n"
first unique character
first non-repeating character
duplicate characters
character frequency
most frequent character
least frequent character
count occurrences
```

think:

```text id="shfy2l"
DICTIONARY / HASH MAP
```

The general pattern is:

```text id="a5a0jg"
COUNT FREQUENCY
      ↓
STORE IN DICTIONARY
      ↓
USE THE COUNTS
```

For this particular problem:

```text id="4cz2e8"
STRING
  ↓
COUNT CHARACTERS
  ↓
SCAN STRING AGAIN
  ↓
count == 1 ?
  ↓
YES
  ↓
RETURN INDEX
```

---

# Quick Interview Explanation

If the interviewer asks how you would solve it:

> I would first use a dictionary to count the frequency of every character. Then I would scan the string from left to right using `enumerate()`. The first character whose frequency is `1` is the first unique character, so I return its index. If no character has a frequency of `1`, I return `-1`.

---

# Pattern to Remember

```text id="uk6jgx"
FIRST UNIQUE CHARACTER

1. COUNT
      ↓
2. DICTIONARY
      ↓
3. LOOP AGAIN
      ↓
4. count == 1?
      ↓
5. RETURN INDEX
```

## Key Formula

```python id="wn0lzu"
char_count[char] = char_count.get(char, 0) + 1
```

Then:

```python id="4jy2ac"
if char_count[char] == 1:
    return index
```

## Key Takeaway

**First loop = count.**

**Second loop = find the first one.**

The main interview pattern is:

**Frequency Counting + Hash Map/Dictionary.**
