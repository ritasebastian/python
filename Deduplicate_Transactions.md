Yes. This is the cleaner **one-pass dictionary approach**. Here's a GitHub-ready version using exactly that pattern.

```python
"""
Deduplicate Transactions - Keep Latest Record

Approach:
- Use txn_id as the dictionary key.
- If txn_id is new, store the transaction.
- If txn_id already exists, compare timestamps.
- Replace the existing record only when the new record is newer.

Time Complexity: O(n)
Space Complexity: O(n)
"""


def deduplicate_transactions(transactions):

    latest = {}

    for txn in transactions:

        txn_id = txn["txn_id"]

        if (
            txn_id not in latest
            or txn["timestamp"] > latest[txn_id]["timestamp"]
        ):
            latest[txn_id] = txn

    return list(latest.values())


transactions = [
    {
        "txn_id": "T101",
        "amount": 500,
        "timestamp": "2026-08-20 10:00:00"
    },
    {
        "txn_id": "T102",
        "amount": 300,
        "timestamp": "2026-08-20 11:00:00"
    },
    {
        "txn_id": "T101",
        "amount": 550,
        "timestamp": "2026-08-20 12:00:00"
    },
    {
        "txn_id": "T102",
        "amount": 350,
        "timestamp": "2026-08-20 13:00:00"
    },
    {
        "txn_id": "T103",
        "amount": 700,
        "timestamp": "2026-08-20 09:00:00"
    }
]


result = deduplicate_transactions(transactions)

for txn in result:
    print(txn)
```

Output:

```text
{'txn_id': 'T101', 'amount': 550, 'timestamp': '2026-08-20 12:00:00'}
{'txn_id': 'T102', 'amount': 350, 'timestamp': '2026-08-20 13:00:00'}
{'txn_id': 'T103', 'amount': 700, 'timestamp': '2026-08-20 09:00:00'}
```

The key line is:

```python
if txn_id not in latest or txn["timestamp"] > latest[txn_id]["timestamp"]:
    latest[txn_id] = txn
```

Read it in English:

> **If I haven't seen this transaction ID before OR this transaction is newer than the one I already have, store this transaction.**

The `or` is especially important. Python uses **short-circuit evaluation** here. If:

```python
txn_id not in latest
```

is `True`, Python doesn't need to evaluate:

```python
latest[txn_id]["timestamp"]
```

So you don't get a `KeyError` for a new `txn_id`.

One production note: these timestamp strings compare correctly because they're consistently formatted as `YYYY-MM-DD HH:MM:SS`. If timestamp formats/time zones can vary, parse them to `datetime` before comparing.
