Yes. Save this GitHub-ready version so later you can quickly remember the **Two Pointers pattern**.

```python id="3e3szx"
"""
Problem: Two Sum in a Sorted Array

Given a sorted array of integers and a target,
find two numbers whose sum equals the target.

Pattern:
- Two Pointers

Approach:
1. Place left pointer at the beginning.
2. Place right pointer at the end.
3. Calculate the sum.
4. If sum is too small, move left forward.
5. If sum is too large, move right backward.
6. If sum equals target, return the two numbers.

Time Complexity: O(n)
Space Complexity: O(1)

Important:
This approach requires the input array to be sorted.
"""


def two_sum(nums, target):

    left = 0
    right = len(nums) - 1

    while left < right:

        current_sum = nums[left] + nums[right]

        if current_sum == target:
            return [nums[left], nums[right]]

        elif current_sum < target:
            left += 1

        else:
            right -= 1

    return []


nums = [1, 2, 3, 4, 6, 8, 9]
target = 12

result = two_sum(nums, target)

print(result)
```

Expected output:

```text
[3, 9]
```

A good filename is:

```text
two_sum_two_pointers.py
```

A good commit message is:

```text
Add two pointers solution for sorted two sum
```

The key interview pattern to remember:

```python
left = 0
right = len(nums) - 1

while left < right:
```

Then decide:

```text
sum too small → left += 1
sum too large → right -= 1
sum == target → found
```
