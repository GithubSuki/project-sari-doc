# Fees API

Fees are service charges to the end-user. Vendors can configure 4 types of fees.

1. Fixed fee
2. Fee based on amount - table and percentage
3. Fee based on distance
4. Conditional Fee to be charged based on payment_type

## Fees Formula

### 1. Fixed Fee

Example:

```json5
{ name: "Delivery Fee", code: "delivery", fee: 100 }
```

### 2. Fee based on Amount

Fees can be calculated using a simple rate table. In this example, fee is P100 (for order amount greater or eualt to 0); and for order amount greater than 1000, fee is P50. There is no limit to the number of row entries in the rate table.

Table Example:

```json5
{   name: "Delivery Fee",
    code: "delivery",
    rates: [
        {amount: 0, fee: 100},
        {amount: 1000, fee 50}
    ]
}
```

Percentage Example:

Delivery fee will be calculated based on 3% of the total amount.

```json5
{ name: "Delivery Fee", code: "delivery", rates: 0.03 }
```

### 3. Fee based on Distance

The example means that fee is 50 pesos for the first 3 kilometers; and every additional kilometer, there is incremental amount or 10 pesos.

- We assume unit of Kilometer and pesos. The confiration is unit-less.

Example:

```json5
{
  name: "Delivery Fee",
  code: "delivery",
  rates: {
    base_distance: 3,
    base_amount: 50,
    additional_distance: 1,
    incremental_amount: 10,
  },
}
```

Sample calculation for above formula.

```js
// 1 km -> 50
// 2 km -> 50
// 3 km -> 50
// 4 km -> 50 + 10 = 60
// 5 km -> 50 + 20 = 70
// 5.5 km round up to 6KM -> 50 + 30 = 80
```

### 4. Conditional Fees

Here is an example of conditional fees. This service charge is applicable only for the payment type 'cash'.

```json5
    {
      name: 'Service Charge',
      fee: 50,
      conditions: { payment_types: ['cash'] },
    },
```

**Express in discount rather than fee.**

It is possible to give discount by specifying a negative fee. Here is an example.

```json5
    {
      name: 'Payment Discount',
      fee: -50,
      conditions: { payment_types: ['online', 'gcash'] },
    },
```

This examples gives pesos discount if the user pays using online and gcash.

\*Note: As of writing, GCASH is not yet supported in the mobile app.

## API for managing fees

### 1. Vendor API

Following API can be use to retrieve (GET) and to update (PUT) fees of vendor.

```
GET /api/current/vendor/fees
PUT /api/current/vendor/fees
```

```json5
{
    delivery: ["formula1", "formula2", ...]
    pickup: ["formula1", "formulat2", ...]
}
```

Just replace formula with the 4 possible formula documented above.

### 2 Branch API

The same API format can be used to manage fees of a branch.

```
GET /api/current/branches/{branch_id}/fees
PUT /api/current/branches/{branch_id}/fees
```

### 3. Locations

The same API format can be used to manage fees of location. Only the end-point is different.

```
GET /api/current/locations/{location_id}/fees
PUT /api/current/locations/{loation_id}/fees
```

# Complete Example

```json5
 {
  pickup: [
    { name: "Picking Fee", fee: 100 },
    {
      name: "Web Fee",
      rates: [
        { amount: 0, fee: 100 },
        { amount: 5000, fee: 0 },
      ],
    },
  ],
  delivery: [
    {
      name: "Delivery Fee",
      calculate: "distance",
      rates: {
        base_distance: 3,
        base_amount: 50,
        incremental_unit: 1,
        incremental_amount: 10,
      },
    },
    {
      name: "Web Fee",
      rates: [
        { amount: 0, fee: 100 },
        { amount: 1000, fee: 0 },
      ],
    },
    {
      name: "Payment Discount",
      fee: -50,
      conditions: { payment_types: ["online", "gcash"] },
    },
  ],
};
```
