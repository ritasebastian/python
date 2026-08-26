
```python
"""
Python List Comprehension and Lambda Examples
Useful reference for Data Engineering coding interviews.
"""


# --------------------------------------------------
# 1. SIMPLE LIST COMPREHENSION
# Pattern: [TRANSFORM for ITEM in LIST]
# --------------------------------------------------

numbers = [10, 20, 30, 40, 50]

doubled = [number * 2 for number in numbers]

print(doubled)
# [20, 40, 60, 80, 100]


# --------------------------------------------------
# 2. FILTER USING LIST COMPREHENSION
# Pattern: [ITEM for ITEM in LIST if CONDITION]
# --------------------------------------------------

greater_than_20 = [
    number
    for number in numbers
    if number > 20
]

print(greater_than_20)
# [30, 40, 50]


# --------------------------------------------------
# 3. FILTER + TRANSFORM
# Pattern: [TRANSFORM for ITEM in LIST if CONDITION]
# --------------------------------------------------

even_numbers_times_10 = [
    number * 10
    for number in numbers
    if number % 2 == 0
]

print(even_numbers_times_10)
# [100, 200, 300, 400, 500]


# --------------------------------------------------
# 4. IF / ELSE LIST COMPREHENSION
# Pattern:
# [TRUE_VALUE if CONDITION else FALSE_VALUE for ITEM in LIST]
# --------------------------------------------------

adjusted = [
    number * 2 if number > 20 else number
    for number in numbers
]

print(adjusted)
# [10, 20, 60, 80, 100]


# --------------------------------------------------
# 5. MULTIPLE CONDITIONS
# --------------------------------------------------

filtered = [
    number
    for number in numbers
    if number >= 20 and number <= 40
]

print(filtered)
# [20, 30, 40]


# Python shortcut for the same range condition:

filtered = [
    number
    for number in numbers
    if 20 <= number <= 40
]


# --------------------------------------------------
# 6. LAMBDA — ACTUAL INLINE / ANONYMOUS FUNCTION
# Pattern: lambda input: output
# --------------------------------------------------

double = lambda number: number * 2

print(double(10))
# 20


# Equivalent regular function:

def double_number(number):
    return number * 2


# --------------------------------------------------
# 7. LAMBDA WITH SORTED()
# Common interview pattern
# --------------------------------------------------

transactions = [
    ("T1", 300),
    ("T2", 100),
    ("T3", 500),
    ("T4", 200),
]

sorted_transactions = sorted(
    transactions,
    key=lambda transaction: transaction[1]
)

print(sorted_transactions)

# [
#     ('T2', 100),
#     ('T4', 200),
#     ('T1', 300),
#     ('T3', 500)
# ]


# --------------------------------------------------
# 8. LAMBDA SORT DESCENDING
# --------------------------------------------------

sorted_transactions = sorted(
    transactions,
    key=lambda transaction: transaction[1],
    reverse=True
)

print(sorted_transactions)

# [
#     ('T3', 500),
#     ('T1', 300),
#     ('T4', 200),
#     ('T2', 100)
# ]


# --------------------------------------------------
# QUICK MEMORY GUIDE
# --------------------------------------------------

# Transform:
# [x * 2 for x in numbers]

# Filter:
# [x for x in numbers if x > 20]

# Filter + Transform:
# [x * 2 for x in numbers if x > 20]

# IF / ELSE:
# [x * 2 if x > 20 else x for x in numbers]

# Multiple conditions:
# [x for x in numbers if x > 10 and x < 50]

# Lambda:
# lambda x: x * 2

# Sort using lambda:
# sorted(items, key=lambda x: x[1])
```

The most important thing for your memory is:

```text
LIST COMPREHENSION
------------------

Transform
[x * 2 for x in numbers]

Filter
[x for x in numbers if condition]

Filter + Transform
[x * 2 for x in numbers if condition]

IF + ELSE
[true_value if condition else false_value for x in numbers]


LAMBDA / INLINE FUNCTION
------------------------

lambda x: x * 2

Regular function:
def double(x):
    return x * 2

Lambda:
lambda x: x * 2
```

