In Python, **decorators** are a way to modify or extend the behavior of functions or methods without changing their actual code. They are often used to add functionality like logging, access control, or performance monitoring in a clean and reusable way.

### Key Concepts
- **Functions are first-class objects in Python:** This means functions can be passed around and used as arguments, just like any other object.
- **A decorator is a function that takes another function as an argument, adds some functionality, and returns the new function.**

### Example for Beginners

Let’s walk through an example step by step.

#### Basic Function
```python
def say_hello():
    print("Hello!")
```

If you call `say_hello()`, it simply prints "Hello!".

#### Adding a Decorator
Let’s create a simple decorator that prints a message before and after calling the `say_hello` function.

1. **Define a decorator function:**
   ```python
   def my_decorator(func):
       def wrapper():
           print("Something is happening before the function is called.")
           func()  # Call the original function
           print("Something is happening after the function is called.")
       return wrapper
   ```

2. **Apply the decorator:**
   Decorators are applied using the `@` symbol.

   ```python
   @my_decorator
   def say_hello():
       print("Hello!")
   ```

   This is equivalent to writing:
   ```python
   say_hello = my_decorator(say_hello)
   ```

3. **Call the decorated function:**
   ```python
   say_hello()
   ```
   Output:
   ```
   Something is happening before the function is called.
   Hello!
   Something is happening after the function is called.
   ```

### Breaking It Down
- `@my_decorator`: This tells Python to pass `say_hello` to the `my_decorator` function.
- Inside `my_decorator`, the `wrapper` function is defined to add extra functionality.
- The original `say_hello` is called inside `wrapper` using `func()`.

### Real-World Example: Logging
Here’s a practical example where decorators are used for logging.

```python
def log_decorator(func):
    def wrapper(*args, **kwargs):
        print(f"Function {func.__name__} is about to be called.")
        result = func(*args, **kwargs)
        print(f"Function {func.__name__} was called.")
        return result
    return wrapper

@log_decorator
def add(a, b):
    return a + b

# Call the function
result = add(5, 3)
print(f"Result: {result}")
```

Output:
```
Function add is about to be called.
Function add was called.
Result: 8
```

### Key Points to Remember
- Decorators modify a function’s behavior without altering its code.
- They can be used for logging, enforcing access control, measuring execution time, and more.
- Use `@decorator_name` to apply a decorator in a clean and readable way.


