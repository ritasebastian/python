Sure. Here’s a complete, GitHub-ready version using the **single-pass aggregation** approach we discussed.

```python
"""
Transaction Summary - Single-Pass Aggregation

Purpose:
    Process transaction records only once and maintain an
    aggregated summary for each customer.

Complexity:
    Aggregation: O(n)
    Memory:      O(k)

    n = number of transactions
    k = number of unique customers
"""

from pprint import pprint


TRANSACTIONS = [
    {
        "id": 101,
        "customer": "A",
        "type": "PAYMENT",
        "amount": 100,
        "status": "SUCCESS",
    },
    {
        "id": 102,
        "customer": "B",
        "type": "PAYMENT",
        "amount": 200,
        "status": "SUCCESS",
    },
    {
        "id": 103,
        "customer": "A",
        "type": "PAYMENT",
        "amount": 50,
        "status": "SUCCESS",
    },
    {
        "id": 104,
        "customer": "C",
        "type": "PAYMENT",
        "amount": 300,
        "status": "SUCCESS",
    },
    {
        "id": 105,
        "customer": "B",
        "type": "REFUND",
        "amount": 40,
        "status": "SUCCESS",
    },
    {
        "id": 106,
        "customer": "A",
        "type": "REFUND",
        "amount": 20,
        "status": "SUCCESS",
    },
    {
        "id": 107,
        "customer": "A",
        "type": "PAYMENT",
        "amount": 500,
        "status": "FAILED",
    },
    {
        "id": 108,
        "customer": "C",
        "type": "CANCEL",
        "amount": 100,
        "status": "SUCCESS",
    },
]


def generate_report(transactions):
    """
    Generate customer-level transaction summaries.

    The input transactions are scanned only once.
    A dictionary is maintained for each unique customer.
    """

    summary = {}

    # Determines how each transaction affects net amount.
    net_sign = {
        "PAYMENT": 1,
        "REFUND": -1,
        "CANCEL": -1,
    }

    # --------------------------------------------
    # SINGLE SCAN OF RAW TRANSACTIONS
    # --------------------------------------------

    for txn in transactions:

        # Ignore unsuccessful transactions.
        if txn["status"] != "SUCCESS":
            continue

        customer = txn["customer"]
        txn_type = txn["type"]
        amount = txn["amount"]

        # Ignore unsupported transaction types.
        if txn_type not in net_sign:
            continue

        # Create customer summary only the first time
        # that customer is encountered.
        data = summary.setdefault(
            customer,
            {
                "payment": 0,
                "refund": 0,
                "cancel": 0,
                "net": 0,
                "count": 0,
                "total_amount": 0,
            },
        )

        # PAYMENT -> payment
        # REFUND  -> refund
        # CANCEL  -> cancel
        category = txn_type.lower()

        # Update transaction-type total.
        data[category] += amount

        # Update net amount.
        data["net"] += amount * net_sign[txn_type]

        # Maintain total transaction amount.
        data["total_amount"] += amount

        # Maintain number of transactions.
        data["count"] += 1

    # --------------------------------------------
    # BUILD FINAL REPORT FROM SMALL SUMMARY
    # --------------------------------------------

    report = []

    for customer, data in summary.items():

        average = (
            data["total_amount"] / data["count"]
            if data["count"]
            else 0
        )

        report.append(
            {
                "customer": customer,
                "payment": data["payment"],
                "refund": data["refund"],
                "cancel": data["cancel"],
                "net": data["net"],
                "count": data["count"],
                "average": round(average, 2),
            }
        )

    # Highest net amount first.
    report.sort(
        key=lambda record: record["net"],
        reverse=True,
    )

    return report


def main():

    report = generate_report(TRANSACTIONS)

    print("\nCustomer Transaction Summary")
    print("-" * 40)

    pprint(report)


if __name__ == "__main__":
    main()
```

The part I'd especially keep for interview/HackerRank practice is this:

```python
data = summary.setdefault(
    customer,
    {
        "payment": 0,
        "refund": 0,
        "cancel": 0,
        "net": 0,
        "count": 0,
        "total_amount": 0,
    },
)
```

Then every time the sequence is:

```text
A → B → A → C → B → A
```

the dictionary behaves conceptually like:

```text
A → create A
B → create B
A → find A → update
C → create C
B → find B → update
A → find A → update
```

No sorting or rescanning of the raw transactions is required.

One interview point to remember: **the aggregation is O(n), but the complete function isn't strictly O(n)** because the final `report.sort()` is O(k log k), where `k` is the number of unique customers. If the interviewer doesn't require sorted output, removing that sort makes the overall processing O(n + k).
