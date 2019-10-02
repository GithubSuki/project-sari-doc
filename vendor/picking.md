# Orders API for Picking App

Some guidelines for the **Picking App**.

1. Server randomly assign an order for picking
2. Only one order should be picked at any given time
3. Vendor portal should have feature to recall the order if the picking takes too long, for example after extended period of time. This could happen in case of human negligence or app crash.

## 1. Fetch Order for Picking

New

```
GET /api/orders?q=get_next_to_pick
```

**Returns:**

- status: NOT_FOUND -> no new order is available
- status: 200 -> Successful

```
{   "_id": 123456789,
    "lines": [
        "product_id": "xxx"
        "name": "xxx"
        "barcode": "12345"
        "quantity": 999
        "wholesale": {
            content_quantity: 99
            quantity: 99
        }
    ],
    "status": "preparing"
}
```

**Notes:**

A new order will be retrieved with status will be changed to pick/started and timestamp recorded.

## 2. Picking Completed

```
PUT /api/orders/{id}/mark_as_picked
```

Body

```
{
  "lines": [
    {
      "product_id": xxx,
      "quantity": xxx
    }
  ]
}
```

**Returns:**

- status: 4xx, 5xx, Error with message
- status: 200, successful

When successful, status will be changed to 'pick/ended' and time will be recorded.

**Notes:**

- Quantities may be edited,
- Zero quantity means lines is to be deleted
- New Items may be added

## 3. Abort Picking

```
PUT /api/orders/{id}/abort_pick
```

## 4. Get New document count

```
GET /api/orders?q=count&status=new
```
