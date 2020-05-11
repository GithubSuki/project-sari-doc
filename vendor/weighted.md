# Weighted Items

How to defined a weighted items or items with fractional value.

## Approxiate Weight and Fixed Price

```json5
{
    barcode: "1"
    name: "Cooked Marinated Chicken Sold Per Pc",
    price: 125,  // fixed price
    weight_info: {  // this is purely informational
        approx_weight: 1.4,
        unit: "KG",
        price_per_unit: 89.28
    },
    quantity: 1,  // always whole number
}
```

## Approxiate Weight and Approx Price

Final Price is calculated based on actual weight

While ordering:

```json5
{
    barcode: "1"
    name: "Cooked Marinated Chicken Sold Per Pc",
    price: 125,  // initial
    weight_info: { // this is still information
        approx_weight: 1.4,
        unit: "KG",
        price_per_unit: 89.28,
        variable: true;
    },
    quantity: 2, // always whole number
    subtotal: 250
}
```

While picking, when actual actual weight is entered:

```json5
{
    barcode: "1"
    name: "Cooked Marinated Chicken Sold Per Pc",
    price: 138.38,  // average price
    weight_info: {  // this is now used to as basic to compute final price when weight is entered
        approx_weight: 1.4,
        unit: "KG",
        price_per_unit: 89.28,
        variable: true;
    },
    quantity: 2, // always whole number
    actual_weights: [1.4, 1.7],  // input during picking
    weight: 3.1,
    subtotal: 276.77
}
```
