Here are simple examples to help beginners understand Python **tuples**:

---

### 1. **Creating a Tuple**
```python
# A tuple of numbers
numbers = (1, 2, 3, 4, 5)
print(numbers)

# A tuple of strings
fruits = ("apple", "banana", "cherry")
print(fruits)

# A mixed tuple
mixed = (1, "apple", 3.14, True)
print(mixed)

# A tuple with one element (comma is necessary!)
single_element = (42,)
print(single_element)
```

---

### 2. **Accessing Elements**
```python
# Access by index
fruits = ("apple", "banana", "cherry")
print(fruits[0])  # Output: apple
print(fruits[1])  # Output: banana
print(fruits[-1]) # Output: cherry (negative index means from the end)
```

---

### 3. **Immutability of Tuples**
```python
# Tuples are immutable; you cannot change their elements
fruits = ("apple", "banana", "cherry")
# fruits[0] = "orange"  # This will raise an error
```

---

### 4. **Slicing Tuples**
```python
numbers = (1, 2, 3, 4, 5, 6)

# Get a sub-tuple
print(numbers[1:4])  # Output: (2, 3, 4)
print(numbers[:3])   # Output: (1, 2, 3)
print(numbers[3:])   # Output: (4, 5, 6)
```

---

### 5. **Looping Through a Tuple**
```python
fruits = ("apple", "banana", "cherry")

# Using a for loop
for fruit in fruits:
    print(fruit)

# Using a while loop
i = 0
while i < len(fruits):
    print(fruits[i])
    i += 1
```

---

### 6. **Checking Membership**
```python
fruits = ("apple", "banana", "cherry")

print("apple" in fruits)  # Output: True
print("orange" in fruits) # Output: False
```

---

### 7. **Tuple Packing and Unpacking**
```python
# Packing
person = ("John", 25, "Engineer")

# Unpacking
name, age, profession = person
print(name)      # Output: John
print(age)       # Output: 25
print(profession) # Output: Engineer
```

---

### 8. **Concatenating Tuples**
```python
tuple1 = (1, 2, 3)
tuple2 = (4, 5, 6)

# Combine tuples using +
combined = tuple1 + tuple2
print(combined)  # Output: (1, 2, 3, 4, 5, 6)
```

---

### 9. **Repeating Tuples**
```python
fruits = ("apple", "banana")

# Repeat elements
repeated = fruits * 3
print(repeated)  # Output: ('apple', 'banana', 'apple', 'banana', 'apple', 'banana')
```

---

### 10. **Finding Length and Index**
```python
fruits = ("apple", "banana", "cherry")

# Length of a tuple
print(len(fruits))  # Output: 3

# Index of an element
print(fruits.index("banana"))  # Output: 1
```

---

### 11. **Count Elements in a Tuple**
```python
numbers = (1, 2, 2, 3, 4, 2)

# Count occurrences of an element
print(numbers.count(2))  # Output: 3
```

---

### 12. **Converting Tuple to List (and Vice Versa)**
```python
# Tuple to List
fruits = ("apple", "banana", "cherry")
fruits_list = list(fruits)
print(fruits_list)  # Output: ['apple', 'banana', 'cherry']

# List to Tuple
fruits_tuple = tuple(fruits_list)
print(fruits_tuple)  # Output: ('apple', 'banana', 'cherry')
```

---

### 13. **Nested Tuples**
```python
nested_tuple = ((1, 2), (3, 4), (5, 6))
print(nested_tuple[1])    # Output: (3, 4)
print(nested_tuple[1][0]) # Output: 3
```

---

### 14. **Using Tuples in Dictionaries**
```python
# Tuples can be used as dictionary keys (immutable)
coordinates = {(1, 2): "Point A", (3, 4): "Point B"}
print(coordinates[(1, 2)])  # Output: Point A
```

---

