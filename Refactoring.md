Sure. Here's the **latest before-vs-after comparison** using your updated tax rates: 0%, 1%, 2%, and 3%.

## Before Refactoring

```python id="k3sz7d"
def calculate_tax(salary):
    if salary < 100:
        return salary
    elif salary < 200:
        return salary - (salary * 0.01)
    elif salary < 300:
        return salary - (salary * 0.02)
    else:
        return salary - (salary * 0.03)
```

## After Refactoring

```python id="l91a3m"
def calculate_tax(salary):
    if salary < 100:
        tax_rate = 0
    elif salary < 200:
        tax_rate = 0.01
    elif salary < 300:
        tax_rate = 0.02
    else:
        tax_rate = 0.03

    tax = salary * tax_rate
    final_salary = salary - tax

    return final_salary
```

## Side-by-side comparison

| Before                                | After                                     |
| ------------------------------------- | ----------------------------------------- |
| Calculation repeated in every branch  | Calculation written once                  |
| `salary - salary * rate` repeated     | Only `tax_rate` changes                   |
| Conditions and calculations are mixed | Conditions determine only the rate        |
| Multiple `return` statements          | One `return`                              |
| Harder to modify calculation          | Easier to modify calculation              |
| Works correctly                       | Works correctly and is easier to maintain |

### What exactly did we refactor?

In the original code, look at the repetition:

```python id="w85u01"
salary - (salary * 0.01)
salary - (salary * 0.02)
salary - (salary * 0.03)
```

Most of the expression is identical:

```python id="p3k9ha"
salary - (salary * ?????)
```

Only this changes:

```text
0.01
0.02
0.03
```

So we extracted the changing part into a variable:

```python id="m06yl4"
tax_rate
```

Then the conditions only decide its value:

```python id="5dl2ic"
if salary < 100:
    tax_rate = 0
elif salary < 200:
    tax_rate = 0.01
elif salary < 300:
    tax_rate = 0.02
else:
    tax_rate = 0.03
```

And the common calculation happens **once**:

```python id="b68x1j"
tax = salary * tax_rate
final_salary = salary - tax

return final_salary
```

### Why are multiple `elif`s still there?

Because they're **not duplicate logic**. Each one represents a different business rule:

```text
< 100       → 0%
100 - 199   → 1%
200 - 299   → 2%
300+        → 3%
```

We shouldn't remove code just to make the program shorter.

The thing we wanted to remove was the **duplicated calculation**, not the necessary business rules.

### Test it

```python id="dz8pr4"
salaries = [50, 150, 250, 350]

for salary in salaries:
    print(salary, "->", calculate_tax(salary))
```

Output:

```text
50  -> 50
150 -> 148.5
250 -> 245.0
350 -> 339.5
```

### Main refactoring lesson

When looking at code, ask:

> **What stays the same, and what changes?**

Here:

```text
Same    → salary - (salary * rate)
Changes → rate
```

So we turn the **changing part into a variable** and write the **common logic only once**.

That's one of the most useful refactoring patterns to learn.
