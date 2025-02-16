Now that we have the **employee badge records grouped** in `badge_log`, we need to **check for proper consecutive enter-exit sequences**. The **rules** are:

1. **First "enter" is always valid** ✅
2. **After "enter", the next action must be "exit"** ✅
3. **If "enter" appears twice in a row, it's invalid** ❌
4. **If "exit" appears twice in a row, it's invalid** ❌
5. **If the sequence is always "enter" → "exit", it's valid** ✅

---

### **Python Code to Check Consecutive Enter-Exit Validity**
```python
from collections import defaultdict

# Step 1: Badge swipe records
data = [("arockia", "enter"), ("arockia", "exit"), 
        ("dave", "exit"), ("david", "enter"), 
        ("arockia", "enter"), ("arockia", "enter"), 
        ("arockia", "exit"), ("dave", "enter"), 
        ("dave", "exit"), ("dave", "exit"), ("david", "exit")]

# Step 2: Store employee badge data
badge_log = defaultdict(list)

for employee, action in data:
    badge_log[employee].append(action)

# Step 3: Define sets to track valid and invalid users
valid_users = set()      # Employees with correct enter-exit sequences
invalid_users = set()    # Employees who break the rules

# Step 4: Check for correct enter-exit sequences
for employee, actions in badge_log.items():
    last_action = None  # Track last action
    valid = True        # Assume valid unless an issue is found

    for action in actions:
        if last_action == "enter" and action == "enter":
            valid = False  # Entered twice in a row (invalid)
        elif last_action == "exit" and action == "exit":
            valid = False  # Exited twice in a row (invalid)

        last_action = action  # Update last action

    if valid:
        valid_users.add(employee)
    else:
        invalid_users.add(employee)

# Step 5: Print results
print("Employees with valid enter-exit sequence:", valid_users)
print("Employees with invalid enter-exit sequence:", invalid_users)
```

---

### **Expected Output**
```python
Employees with valid enter-exit sequence: {'david'}
Employees with invalid enter-exit sequence: {'arockia', 'dave'}
```

---

### **Explanation**
1. **✅ Valid: `"david"`**
   - `"enter"` → `"exit"` (Correct)
   
2. **❌ Invalid: `"arockia"`**
   - `"enter"` → `"enter"` (Wrong, must be `"exit"` after first `"enter"`)

3. **❌ Invalid: `"dave"`**
   - `"exit"` → `"enter"` → `"exit"` → `"exit"` (Double `"exit"` is wrong)

---

### **How This Works**
1. We loop through each employee's records.
2. Check that **every "enter" is followed by "exit"**.
3. If `"enter"` appears **twice in a row**, mark as **invalid**.
4. If `"exit"` appears **twice in a row**, mark as **invalid**.
5. Store valid users in `valid_users` and invalid users in `invalid_users`.

This **ensures a proper badge sequence**! 🚀 Let me know if you have any questions. 😊
