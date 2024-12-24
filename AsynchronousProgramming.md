Asynchronous programming in Python allows you to perform tasks without blocking the execution of your program. This is particularly useful for I/O-bound operations like reading from a file, making network requests, or interacting with a database.

Python provides the `asyncio` module to write asynchronous code using `async` and `await` keywords.

### Example: Asynchronous Programming in Python

Here's an example that demonstrates asynchronous programming:

```python
import asyncio
import random

async def task(name, duration):
    print(f"{name}: Started. Will take {duration} seconds.")
    await asyncio.sleep(duration)  # Simulate an asynchronous I/O operation
    print(f"{name}: Completed.")

async def main():
    # Create multiple tasks
    tasks = [
        task("Task 1", random.randint(1, 5)),
        task("Task 2", random.randint(1, 5)),
        task("Task 3", random.randint(1, 5))
    ]
    # Run tasks concurrently
    await asyncio.gather(*tasks)

# Run the main function
asyncio.run(main())
```

### Output (example, actual times will vary due to randomness)
```
Task 1: Started. Will take 3 seconds.
Task 2: Started. Will take 2 seconds.
Task 3: Started. Will take 4 seconds.
Task 2: Completed.
Task 1: Completed.
Task 3: Completed.
```

### Explanation:

1. **`async` functions**:
   - Defined using `async def`.
   - Can be paused using `await` to allow other tasks to run.

2. **`await`**:
   - Used to pause the execution of the current task until the awaited task completes.

3. **`asyncio.sleep`**:
   - Simulates an asynchronous delay. Unlike `time.sleep`, it doesn't block the entire program.

4. **`asyncio.gather`**:
   - Runs multiple asynchronous tasks concurrently.

5. **`asyncio.run`**:
   - Entry point to run the asynchronous program.

### Benefits:
- Efficiently handles I/O-bound tasks.
- Enables concurrent execution, leading to faster completion times for multiple operations. 

This approach doesn't improve performance for CPU-bound tasks; for those, you may want to use multiprocessing instead.
