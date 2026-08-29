Your cleaning/lookup functions are good now. Here is the **complete refactored version**, keeping your approach rather than changing everything to comprehensions.

```python
def clean_customer(customers):
    seen_ids = set()
    clean_customer_records = []

    for customer in customers:
        customer_id = customer.get("id")
        customer_status = customer.get("status")

        if customer_id not in seen_ids and customer_status == "active":
            seen_ids.add(customer_id)
            clean_customer_records.append(customer)

    return clean_customer_records


def lookup_customer(clean_customer_records):
    customer_lookup = {}

    for customer in clean_customer_records:
        customer_id = customer.get("id")
        customer_lookup[customer_id] = customer

    return customer_lookup


def clean_payment(payments):
    clean_payment_records = []

    for payment in payments:
        status = payment.get("payment_status")

        if status == "paid":
            clean_payment_records.append(payment)

    return clean_payment_records


def lookup_payment(clean_payment_records):
    payment_lookup = {}

    for payment in clean_payment_records:
        order_id = payment.get("order_id")
        payment_lookup[order_id] = payment

    return payment_lookup


def clean_order(orders):
    clean_order_records = []

    for order in orders:
        status = order.get("status")

        if status == "completed":
            clean_order_records.append(order)

    return clean_order_records


def get_category(total_amount):

    if total_amount >= 1000:
        return "PLATINUM"

    elif total_amount >= 500:
        return "GOLD"

    elif total_amount >= 200:
        return "SILVER"

    else:
        return "BRONZE"


customers = [
    {"id": 101, "name": "John", "email": "john@gmail.com", "country": "US", "status": "active"},
    {"id": 102, "name": "Mary", "email": "mary@gmail.com", "country": "CA", "status": "active"},
    {"id": 101, "name": "John", "email": "john@gmail.com", "country": "US", "status": "active"},
    {"id": 103, "name": "David", "email": "david@gmail.com", "country": "US", "status": "inactive"},
    {"id": 104, "name": "Lisa", "email": "lisa@gmail.com", "country": "IN", "status": "active"},
    {"id": 105, "name": "Sam", "email": "sam@gmail.com", "country": "US", "status": "active"},
    {"id": 102, "name": "Mary", "email": "mary@gmail.com", "country": "CA", "status": "active"},
]


orders = [
    {"order_id": 5001, "customer_id": 101, "amount": 250, "status": "completed"},
    {"order_id": 5002, "customer_id": 102, "amount": 500, "status": "completed"},
    {"order_id": 5003, "customer_id": 101, "amount": 100, "status": "cancelled"},
    {"order_id": 5004, "customer_id": 104, "amount": 700, "status": "completed"},
    {"order_id": 5005, "customer_id": 105, "amount": 300, "status": "completed"},
    {"order_id": 5006, "customer_id": 101, "amount": 450, "status": "completed"},
    {"order_id": 5007, "customer_id": 102, "amount": 150, "status": "completed"},
    {"order_id": 5008, "customer_id": 999, "amount": 1000, "status": "completed"},
]


payments = [
    {"order_id": 5001, "payment_status": "paid"},
    {"order_id": 5002, "payment_status": "paid"},
    {"order_id": 5004, "payment_status": "failed"},
    {"order_id": 5005, "payment_status": "paid"},
    {"order_id": 5006, "payment_status": "paid"},
    {"order_id": 5007, "payment_status": "paid"},
]


# ==================================================
# STEP 1 - CLEAN DATA
# ==================================================

clean_customer_records = clean_customer(customers)
clean_payment_records = clean_payment(payments)
clean_order_records = clean_order(orders)


# ==================================================
# STEP 2 - CREATE LOOKUPS
# ==================================================

customer_lookup = lookup_customer(clean_customer_records)
payment_lookup = lookup_payment(clean_payment_records)


# ==================================================
# STEP 3 - PROCESS VALID ORDERS
# ==================================================

customer_summary = {}


for order in clean_order_records:

    order_id = order.get("order_id")
    customer_id = order.get("customer_id")

    # Lookup active customer
    customer = customer_lookup.get(customer_id)

    if customer is None:
        continue

    # Lookup paid payment
    payment = payment_lookup.get(order_id)

    if payment is None:
        continue

    # ----------------------------------------------
    # Customer has valid completed + paid order
    # ----------------------------------------------

    if customer_id not in customer_summary:

        customer_summary[customer_id] = {
            "customer_id": customer_id,
            "customer_name": customer.get("name"),
            "email": customer.get("email"),
            "country": customer.get("country"),
            "number_of_orders": 0,
            "total_amount": 0
        }

    customer_summary[customer_id]["number_of_orders"] += 1

    customer_summary[customer_id]["total_amount"] += order.get("amount", 0)


# ==================================================
# STEP 4 - CALCULATE AVERAGE + CATEGORY
# ==================================================

result = []


for customer in customer_summary.values():

    total_amount = customer["total_amount"]
    number_of_orders = customer["number_of_orders"]

    average_order_amount = total_amount / number_of_orders

    category = get_category(total_amount)

    customer["average_order_amount"] = average_order_amount
    customer["category"] = category

    result.append(customer)


# ==================================================
# STEP 5 - SORT
# ==================================================

result = sorted(
    result,
    key=lambda customer: customer["total_amount"],
    reverse=True
)


# ==================================================
# STEP 6 - PRINT REPORT
# ==================================================

for customer in result:

    print("----------------------------")
    print("Customer ID:", customer["customer_id"])
    print("Name:", customer["customer_name"])
    print("Country:", customer["country"])
    print("Orders:", customer["number_of_orders"])
    print("Total:", customer["total_amount"])
    print("Average:", customer["average_order_amount"])
    print("Category:", customer["category"])
```

The output is:

```text
----------------------------
Customer ID: 101
Name: John
Country: US
Orders: 2
Total: 700
Average: 350.0
Category: GOLD

----------------------------
Customer ID: 102
Name: Mary
Country: CA
Orders: 2
Total: 650
Average: 325.0
Category: GOLD

----------------------------
Customer ID: 105
Name: Sam
Country: US
Orders: 1
Total: 300
Average: 300.0
Category: SILVER
```

Notice **Lisa (104) disappears** even though she has a completed `$700` order.

That's because:

```python
order 5004 → completed
customer 104 → active
payment 5004 → failed
```

`clean_payment()` removes failed payments, so:

```python
payment_lookup.get(5004)
```

returns `None`, and we skip it.

Also `5008` disappears because customer `999` isn't in `customer_lookup`.

The most important part of your refactoring is now this:

```python
for order in clean_order_records:

    order_id = order.get("order_id")
    customer_id = order.get("customer_id")

    customer = customer_lookup.get(customer_id)

    if customer is None:
        continue

    payment = payment_lookup.get(order_id)

    if payment is None:
        continue
```

That replaces most of the ugly nested customer → order → payment searching in the original code.
