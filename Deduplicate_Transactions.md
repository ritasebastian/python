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
