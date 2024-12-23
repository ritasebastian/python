In Python, an **iterator** is an object that allows you to iterate over a sequence of elements, such as a list, tuple, or string, one element at a time. It follows the **iterator protocol**, which consists of two methods:

- `__iter__()`: Returns the iterator object itself.
- `__next__()`: Returns the next element in the sequence. If there are no more items, it raises a `StopIteration` exception.

### Simple Example of Iterators

Here's a basic example to demonstrate iterators:

```python
# Example: Using an iterator explicitly

# A list of numbers
numbers = [1, 2, 3, 4, 5]

# Get an iterator from the list
iterator = iter(numbers)

# Use the iterator to access elements
print(next(iterator))  # Output: 1
print(next(iterator))  # Output: 2
print(next(iterator))  # Output: 3
print(next(iterator))  # Output: 4
print(next(iterator))  # Output: 5

# Trying to call next() again will raise StopIteration
# print(next(iterator))  # Uncommenting this will cause an error
```

### Simplified Use with a `for` Loop

Python's `for` loop automatically handles iterators for you:

```python
# The same list
numbers = [1, 2, 3, 4, 5]

# The for loop implicitly creates an iterator
for num in numbers:
    print(num)
```

### Custom Iterator Example

You can also create your own iterator by defining a class with `__iter__()` and `__next__()` methods:

```python
class Counter:
    def __init__(self, start, end):
        self.current = start
        self.end = end

    def __iter__(self):
        return self  # The iterator object itself

    def __next__(self):
        if self.current > self.end:
            raise StopIteration  # Stop the iteration
        else:
            self.current += 1
            return self.current - 1

# Create an iterator for numbers 1 to 5
counter = Counter(1, 5)

# Use it in a loop
for num in counter:
    print(num)
```

Output:

```
1
2
3
4
5
```

### Key Points
- **Iterable**: An object that can return an iterator using `iter()`.
- **Iterator**: An object with `__iter__()` and `__next__()` methods.
- **`for` Loops**: They simplify iteration by handling the iterator protocol behind the scenes. 

