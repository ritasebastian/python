Here’s an extended list of additional Python challenge programs for testing and improving your problem-solving and coding skills:

---

### 11. **Check if a String Contains Only Digits**

```python
def is_digit_only(s):
    return s.isdigit()

print(is_digit_only("12345"))  # True
print(is_digit_only("123a5"))  # False
```

---

### 12. **Find the Largest Element in a List**

```python
nums = [3, 5, 7, 2, 8]
largest = max(nums)
print(largest)  # 8
```

---

### 13. **Sum of Digits in an Integer**

```python
def sum_of_digits(n):
    return sum(int(d) for d in str(n))

print(sum_of_digits(123))  # 6
```

---

### 14. **Count the Frequency of Characters in a String**

```python
from collections import Counter

def char_frequency(s):
    return Counter(s)

print(char_frequency("hello"))  # {'h': 1, 'e': 1, 'l': 2, 'o': 1}
```

---

### 15. **Check for a Substring in a String**

```python
def contains_substring(string, substring):
    return substring in string

print(contains_substring("hello world", "world"))  # True
print(contains_substring("hello world", "Python"))  # False
```

---

### 16. **Find All Prime Numbers in a Range**

```python
def primes_in_range(start, end):
    primes = []
    for num in range(start, end + 1):
        if all(num % i != 0 for i in range(2, int(num**0.5) + 1)) and num > 1:
            primes.append(num)
    return primes

print(primes_in_range(10, 50))
```

---

### 17. **Find the GCD of Two Numbers**

```python
import math

def gcd(a, b):
    return math.gcd(a, b)

print(gcd(24, 36))  # 12
```

---

### 18. **Check for Armstrong Numbers in a Range**

```python
def armstrong_in_range(start, end):
    return [n for n in range(start, end + 1) if n == sum(int(d)**len(str(n)) for d in str(n))]

print(armstrong_in_range(1, 1000))  # [1, 153, 370, 371, 407]
```

---

### 19. **Remove Duplicates from a List**

```python
nums = [1, 2, 2, 3, 4, 4, 5]
unique_nums = list(set(nums))
print(unique_nums)  # [1, 2, 3, 4, 5]
```

---

### 20. **Transpose a Matrix**

```python
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
transpose = [[row[i] for row in matrix] for i in range(len(matrix[0]))]
print(transpose)
```

---

### 21. **Check if a Number is Perfect**

A perfect number is one that is equal to the sum of its divisors (excluding itself).

```python
def is_perfect(num):
    divisors = [i for i in range(1, num) if num % i == 0]
    return sum(divisors) == num

print(is_perfect(28))  # True
print(is_perfect(10))  # False
```

---

### 22. **Binary Search Implementation**

```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1

print(binary_search([1, 2, 3, 4, 5], 3))  # 2
```

---

### 23. **Check if Two Strings are Rotations of Each Other**

```python
def are_rotations(s1, s2):
    return len(s1) == len(s2) and s2 in s1 + s1

print(are_rotations("abcd", "cdab"))  # True
print(are_rotations("abcd", "dabc"))  # True
print(are_rotations("abcd", "abdc"))  # False
```

---

### 24. **Find Second Largest Number in a List**

```python
nums = [10, 20, 4, 45, 99]
unique_nums = list(set(nums))
unique_nums.sort()
print(unique_nums[-2])  # 45
```

---

### 25. **Count Vowels in a String**

```python
def count_vowels(s):
    vowels = "aeiou"
    return sum(1 for char in s.lower() if char in vowels)

print(count_vowels("hello world"))  # 3
```

---

### 26. **Check if a List is Sorted**

```python
def is_sorted(nums):
    return nums == sorted(nums)

print(is_sorted([1, 2, 3, 4]))  # True
print(is_sorted([3, 1, 4, 2]))  # False
```

---

### 27. **Generate a Random Password**

```python
import random
import string

def generate_password(length):
    chars = string.ascii_letters + string.digits + string.punctuation
    return ''.join(random.choice(chars) for _ in range(length))

print(generate_password(10))
```

---

### 28. **Find the Longest Word in a String**

```python
def longest_word(sentence):
    words = sentence.split()
    return max(words, key=len)

print(longest_word("Python is an amazing programming language"))  # programming
```

---

### 29. **Find the Sum of a List Without Using `sum()`**

```python
nums = [1, 2, 3, 4, 5]
total = 0
for num in nums:
    total += num
print(total)  # 15
```

---

### 30. **Flatten a Nested List**

```python
def flatten_list(nested):
    return [item for sublist in nested for item in sublist]

nested_list = [[1, 2], [3, 4], [5]]
print(flatten_list(nested_list))  # [1, 2, 3, 4, 5]
```

---

Let me know which program you'd like to explore in detail, or feel free to ask for more!
