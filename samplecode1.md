To properly **track multiple enter-exit cycles** for each employee, we need to ensure:
1. **Every "enter" has a corresponding "exit"** before another "enter" happens.
2. **Every "exit" has a corresponding "enter"** before another "exit" happens.
3. Employees who maintain a **proper cycle** throughout are marked as **valid**.
4. Employees who break the enter-exit sequence are **invalid**.

---

## **🔹 Updated Approach**
- Maintain a **count of open enters** (entries without exits).
- Track if **any sequence is broken** (like consecutive enters or exits).
- If an employee **always alternates between enter-exit** correctly, they are valid.

---

### **✅ Updated Python Code**
```python
from collections import defaultdict

# Step 1: Badge swipe records
data = [("arockia", "enter"), ("arockia", "exit"),
        ("dave", "exit"), ("david", "enter"),
        ("arockia", "enter"), ("arockia", "enter"),
        ("arockia", "exit"), ("dave", "enter"),
        ("dave", "exit"), ("dave", "exit"), ("david", "exit"),
        ("john", "enter"), ("john", "exit"), ("john", "enter"), ("john", "exit")]  # Proper multiple enter-exit

# Step 2: Store employee badge data
badge_log = defaultdict(list)

for employee, action in data:
    badge_log[employee].append(action)

# Step 3: Define sets to track valid and invalid users
valid_users = set()      # Employees with correct enter-exit sequences
invalid_users = set()    # Employees who break the enter-exit pattern

# Step 4: Check for proper multiple enter-exit sequences
for employee, actions in badge_log.items():
    open_enters = 0  # Tracks number of enters that are not exited
    valid = True     # Assume valid unless an issue is found

    for action in actions:
        if action == "enter":
            if open_enters > 0:  # If already entered before an exit
                valid = False  # Mark as invalid
            open_enters += 1
        elif action == "exit":
            if open_enters == 0:  # If exiting before entering
                valid = False  # Mark as invalid
            else:
                open_enters -= 1  # Reduce the open enters count

    if valid and open_enters == 0:  # Valid only if all enters have exits
        valid_users.add(employee)
    else:
        invalid_users.add(employee)

# Step 5: Print results
print("Employees with valid multiple enter-exit sequences:", valid_users)
print("Employees with invalid enter-exit sequences:", invalid_users)
```

---

### **🔹 Expected Output**
For the given **badge swipes**, the output will be:
```python
Employees with valid multiple enter-exit sequences: {'david', 'john'}
Employees with invalid enter-exit sequences: {'arockia', 'dave'}
```

---

### **🔹 Explanation**
✅ **Valid Employees:**
1. **`david`**:
   - `["enter", "exit"]` ✅ **Correct pattern**
2. **`john`**:
   - `["enter", "exit", "enter", "exit"]` ✅ **Multiple enter-exit cycles (valid)**

❌ **Invalid Employees:**
1. **`arockia`**:
   - `["enter", "exit", "enter", "enter", "exit"]`
   - `"enter", "enter"` ❌ **Entered twice before exiting** (Invalid)
2. **`dave`**:
   - `["exit", "enter", "exit", "exit"]`
   - `"exit"` **before any enter** ❌ **(Invalid)**

---

## **🔹 How This Works**
1. **Tracks open "enters"** using `open_enters` to check if all entries have matching exits.
2. **Ensures no consecutive enters/exits**.
3. **Marks users valid only if they maintain a correct enter-exit cycle**.

Now, this code **correctly validates multiple enter-exit cycles** for each employee! 🚀  
Let me know if you need further explanation! 😊
