Here’s a simple **Python example using `ijson`** to read a large JSON file efficiently (without loading the entire file into memory):

---

### ✅ Use Case:

Parse a large JSON file (e.g., array of records), and print only those records where a condition matches (e.g., `user['age'] > 30`).

---

### 🛠️ Step-by-Step Code:

```python
import ijson

# Sample JSON structure:
# [
#   {"name": "Alice", "age": 25},
#   {"name": "Bob", "age": 35},
#   {"name": "Charlie", "age": 40}
# ]

filename = 'sample.json'

with open(filename, 'r') as f:
    # 'item' is each object inside the top-level array
    objects = ijson.items(f, 'item')

    for user in objects:
        if user.get('age', 0) > 30:
            print(user)
```

---

### 📦 Install `ijson`

```bash
pip install ijson
```

---

### 🔍 How it works:

* `ijson.items(f, 'item')` parses the file as a stream.
* Efficient for large files (doesn’t load all into memory like `json.load`).
* `item` refers to each object inside the top-level list (array of JSON objects).

---


