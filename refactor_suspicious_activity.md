from collections import defaultdict, deque


def find_suspicious_transactions(transactions):

    # Each account_id gets its own deque
    # Example:
    # A1 -> deque([10, 15, 18])
    # A2 -> deque([16])
    windows = defaultdict(deque)

    suspicious = []

    for txn in transactions:

        account_id = txn["account_id"]
        current_time = txn["timestamp"]

        # Get the deque for the current account
        window = windows[account_id]

        # Remove timestamps older than 10 seconds
        while window and current_time - window[0] > 10:
            window.popleft()

        # Add current timestamp
        window.append(current_time)

        # If this account has 3 or more transactions
        # within the 10-second window, flag current transaction
        if len(window) >= 3:
            suspicious.append(txn)

    return suspicious


transactions = [
    {"account_id": "A1", "timestamp": 10, "amount": 100},
    {"account_id": "A1", "timestamp": 15, "amount": 200},
    {"account_id": "A2", "timestamp": 16, "amount": 50},
    {"account_id": "A1", "timestamp": 18, "amount": 300},
    {"account_id": "A1", "timestamp": 30, "amount": 400},
]


result = find_suspicious_transactions(transactions)

print(result)

# Detect Suspicious Transactions Using a Sliding Time Window

## Problem

Given a list of transactions, detect transactions that belong to a suspicious activity spike.

Each transaction contains:

* `account_id` — identifies the account
* `amount` — transaction amount
* `timestamp` — time when the transaction occurred

A transaction group is considered suspicious when the **same account has more than 2 transactions within a 10-second window**.

Return the **original indices** of all transactions that belong to a suspicious window.

---

## Example

### Input

```python
transactions = [
    {"account_id": 101, "amount": 50.0, "timestamp": 100},
    {"account_id": 102, "amount": 20.0, "timestamp": 101},
    {"account_id": 101, "amount": 15.0, "timestamp": 105},
    {"account_id": 101, "amount": 99.0, "timestamp": 108},
    {"account_id": 102, "amount": 10.0, "timestamp": 120},
]
```

For account `101`:

```text
Index       Timestamp
---------------------
0           100
2           105
3           108
```

Check the time window:

```text
108 - 100 = 8 seconds
```

All three transactions occur within 10 seconds.

Since:

```text
3 transactions > 2 allowed
```

the transactions at indices:

```text
0, 2, 3
```

are suspicious.

### Output

```python
[0, 2, 3]
```

---

# Approach

We maintain a separate sliding window for every account.

Conceptually:

```text
windows = {

    101: [(timestamp, index), ...],

    102: [(timestamp, index), ...],

    103: [(timestamp, index), ...]

}
```

For every incoming transaction:

```text
1. Find its account
        ↓
2. Add it to that account's window
        ↓
3. Remove transactions older than 10 seconds
        ↓
4. Count transactions remaining in the window
        ↓
5. If count > 2
        ↓
6. Flag all indices in the window
```

---

# Python Solution

```python
def detect_suspicious_transactions(
    transactions,
    window_size=10,
    max_allowed=2
):

    # Stores the active transaction window for each account.
    #
    # Example:
    # 101 -> [(100, 0), (105, 2), (108, 3)]
    windows = {}

    # Set prevents duplicate indices.
    flagged = set()

    # Process transactions one at a time.
    for index, txn in enumerate(transactions):

        account = txn["account_id"]
        current_time = txn["timestamp"]

        # Create a window when we encounter
        # an account for the first time.
        if account not in windows:
            windows[account] = []

        # Add current transaction.
        #
        # Store:
        # (timestamp, original_index)
        windows[account].append(
            (current_time, index)
        )

        # Build the active 10-second window.
        new_window = []

        for old_time, old_index in windows[account]:

            # Keep transactions that are still
            # within the allowed time window.
            if current_time - old_time <= window_size:

                new_window.append(
                    (old_time, old_index)
                )

        # Replace old window with active window.
        windows[account] = new_window

        # If the number of transactions exceeds
        # the allowed threshold, flag all of them.
        if len(windows[account]) > max_allowed:

            for old_time, old_index in windows[account]:
                flagged.add(old_index)

    # Return original indices in sorted order.
    return sorted(flagged)
```

---

# Test

```python
transactions = [
    {"account_id": 101, "amount": 50.0, "timestamp": 100},
    {"account_id": 102, "amount": 20.0, "timestamp": 101},
    {"account_id": 101, "amount": 15.0, "timestamp": 105},
    {"account_id": 101, "amount": 99.0, "timestamp": 108},
    {"account_id": 102, "amount": 10.0, "timestamp": 120},
]

result = detect_suspicious_transactions(transactions)

print(result)
```

### Output

```text
[0, 2, 3]
```

---

# How the Window Changes

For account `101`:

### Transaction 1

```text
timestamp = 100

window:

[100]

count = 1

Not suspicious
```

### Transaction 2

```text
timestamp = 105

window:

[100, 105]

105 - 100 = 5

count = 2

Not suspicious
```

### Transaction 3

```text
timestamp = 108

window:

[100, 105, 108]

108 - 100 = 8

count = 3

3 > 2

SUSPICIOUS
```

Flag:

```text
[0, 2, 3]
```

---

# What Happens When a Transaction Becomes Too Old?

Suppose account `101` later has a transaction at:

```text
timestamp = 115
```

Before removing old transactions:

```text
[100, 105, 108, 115]
```

Check `100`:

```text
115 - 100 = 15
```

Since:

```text
15 > 10
```

timestamp `100` is outside the window and is removed.

Check `105`:

```text
115 - 105 = 10
```

It is still inside the window.

The new active window becomes:

```text
[105, 108, 115]
```

This is why the technique is called a **sliding window**.

The window moves forward as time moves forward.

---

# Why Use a Dictionary?

Transactions for different accounts can be mixed:

```text
101
102
103
101
104
101
```

The account `101` transactions do not need to be next to each other.

The dictionary remembers a separate window for every account:

```text
101 -> [...]
102 -> [...]
103 -> [...]
104 -> [...]
```

When another transaction for account `101` appears, we simply access:

```python
windows[101]
```

and continue updating its existing window.

---

# Why Use a Set for `flagged`?

The same transaction may appear in more than one suspicious window.

For example:

```text
100, 105, 108
```

may be suspicious.

Then:

```text
105, 108, 110
```

may also be suspicious.

Some indices would be added more than once.

Using:

```python
flagged = set()
```

automatically prevents duplicates.

---

# Important Assumption

This implementation assumes transactions are processed in **nondecreasing timestamp order**.

For example:

```text
100
101
105
108
120
```

If the input can arrive out of chronological order, sort it first while preserving the original index.

A common approach is:

```python
data = list(enumerate(transactions))

data.sort(
    key=lambda x: (
        x[1]["account_id"],
        x[1]["timestamp"]
    )
)
```

Then apply the sliding-window logic.

---

# Complexity

The outer loop processes each incoming transaction once.

However, this beginner-friendly implementation also scans the current account's window:

```python
for old_time, old_index in windows[account]:
```

and may scan it again when flagging transactions.

Therefore, although the input is processed sequentially, the implementation is **not guaranteed to be O(N)** in the worst case.

For small windows this can be perfectly reasonable and is easy to understand.

For a more optimized implementation, use:

```python
collections.deque
```

so expired transactions can be removed efficiently from the front of each account's window.

---

# Interview Pattern

When a problem says:

```text
"within 10 seconds"
"within 5 minutes"
"last N seconds"
"within a time range"
```

think:

**SLIDING WINDOW**

When it also says:

```text
"for each account"
"for each customer"
"for each user"
```

think:

**DICTIONARY + SLIDING WINDOW**

So the pattern for this problem is:

```text
Transaction
     ↓
Identify Account
     ↓
Get Account's Window
     ↓
Add Current Transaction
     ↓
Remove Expired Transactions
     ↓
Check Window Size
     ↓
Flag Suspicious Transactions
```

## Key Takeaway

**Window = the transactions that are relevant right now.**

**Sliding window = add new transactions and remove transactions that have become too old.**
