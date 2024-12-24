

### 1. **List**
   - **Mutable**: You can modify the elements (add, remove, or change).
   - Example:
     ```python
     # Creating a list
     my_list = [1, 2, 3, 4]
     
     # Modifying the list
     my_list.append(5)        # Adds 5
     my_list[0] = 10          # Changes the first element to 10
     print(my_list)           # Output: [10, 2, 3, 4, 5]
     ```

---

### 2. **Tuple**
   - **Immutable**: You cannot change the elements after creation.
   - Example:
     ```python
     # Creating a tuple
     my_tuple = (1, 2, 3)
     
     # Trying to modify
     # my_tuple[0] = 10  # Raises TypeError
     
     print(my_tuple)       # Output: (1, 2, 3)
     ```

---

### 3. **Set**
   - **Mutable**: You can add or remove elements, but no duplicates are allowed.
   - Example:
     ```python
     # Creating a set
     my_set = {1, 2, 3}
     
     # Modifying the set
     my_set.add(4)         # Adds 4
     my_set.remove(2)      # Removes 2
     print(my_set)         # Output: {1, 3, 4}
     ```

---

### 4. **Dictionary**
   - **Mutable**: Keys and values can be modified or removed.
   - Example:
     ```python
     # Creating a dictionary
     my_dict = {'a': 1, 'b': 2}
     
     # Modifying the dictionary
     my_dict['c'] = 3         # Adds a new key-value pair
     my_dict['a'] = 10        # Modifies the value for key 'a'
     del my_dict['b']         # Removes the key 'b'
     print(my_dict)           # Output: {'a': 10, 'c': 3}
     ```

---

### 5. **String**
   - **Immutable**: You cannot modify the string in place.
   - Example:
     ```python
     # Creating a string
     my_string = "hello"
     
     # Trying to modify
     # my_string[0] = 'H'  # Raises TypeError
     
     # Reassign to create a new string
     my_string = "Hello"
     print(my_string)        # Output: Hello
     ```

---

### 6. **Range**
   - **Immutable**: Represents a sequence of numbers but cannot be modified.
   - Example:
     ```python
     # Creating a range
     my_range = range(1, 5)
     
     # Accessing range elements
     print(list(my_range))   # Output: [1, 2, 3, 4]
     ```

---

### 7. **Bytes**
   - **Immutable**: A sequence of bytes that cannot be modified.
   - Example:
     ```python
     # Creating bytes
     my_bytes = b"hello"
     
     # Trying to modify
     # my_bytes[0] = b'H'  # Raises TypeError
     print(my_bytes)        # Output: b'hello'
     ```

---

### 8. **Bytearray**
   - **Mutable**: Similar to bytes but allows modification.
   - Example:
     ```python
     # Creating bytearray
     my_bytearray = bytearray(b"hello")
     
     # Modifying the bytearray
     my_bytearray[0] = ord('H')
     print(my_bytearray)    # Output: bytearray(b'Hello')
     ```

---

### Summary Table

| **Data Structure** | **Mutable**  | **Example**                    |
|---------------------|--------------|---------------------------------|
| List               | ✅ Mutable   | `[1, 2, 3]`                    |
| Tuple              | ❌ Immutable | `(1, 2, 3)`                    |
| Set                | ✅ Mutable   | `{1, 2, 3}`                    |
| Dictionary         | ✅ Mutable   | `{'a': 1, 'b': 2}`             |
| String             | ❌ Immutable | `"hello"`                      |
| Range              | ❌ Immutable | `range(1, 5)`                  |
| Bytes              | ❌ Immutable | `b"hello"`                     |
| Bytearray          | ✅ Mutable   | `bytearray(b"hello")`          |

