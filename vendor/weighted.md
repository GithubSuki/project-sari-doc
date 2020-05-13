# Weighted Items

Suki Commerce supports weighted items such as fresh produce in additional to regular items. User can store weight information such as `approx_weight`, `unit_in_weight`, and `price_per_unit`.

There are two type of weighted items. One that has fixed price, and another price based on actual weight during picking.

## Upoading Weight Info

Weight information can be uploaded to the system using CSV formatted file. Here is an example:

### A. Fixed Price

Let say Marinated Chicken (barcode 111) is to be sold at fixed price of P140 per pc, with average weight of 1.4KG,

- Item Name: Marinated Chicken
- Barcode: 111
- Price: 140
- Approximate Weight: 1.4
- Unit in weight: KG
- Price / KG: 100

This is how your CSV file should look like.

```
barcode,price,approx_weight,unit_in_weight,price_per_unit
111,1.4,KG,100
```

### B. Variable Price

If you would allow the price to be adjusted based on actual weight, all you need to do is to include another column `variable`.

```
barcode,price,approx_weight,unit_in_weight,price_per_unit,variable
111,1.4,KG,100,true
```

## Data Structure

Internally, this is how the information is being store. Weigh info is kept as sub-document within the product document.

```json5
{
    barcode: "111"
    name: "Cooked Marinated Chicken",
    price: 140,
    weight_info: {
        approx_weight: 1.4,
        unit: "KG",
        price_per_unit: 100,
        variable: false
    }
}
```

## Mobile Client

Mobile App is responsible to present the weight info the to user. When downloading or searching, weighted items will the weigh_info included as part of JSON.

#### Fixed Price Item

Image 1. Below is sample screen showing how MetroMart App renders fixed price item.

![Fixed Price](/images/weighted_fixed_price.jpg =200x)

#### Variable Price Item

Image 2: Below is an example showing how Metromart App renders variable price item.

![Variable Price](/images/weighted_variable_price.jpg =200x)

<img src="/images/weighted_variable_price.jpg" alt="var price"/>

#### Submitting Order

When submitting order, there is no difference in how regular items are submitted versus an weighte items. There is no need to include the weigh info in submission, Suki server automatically detects the weighted item and include them in the final order.

During submission:

```json5
{
  lines: [
    {
      barcode: 111,
      quantity: 1,
    },
  ],
}
```

After submission:

```json5
{   lines: [{
        barcode: "111"
        name: "Cooked Marinated Chicken",
        quantity: 1,
        price: 140,  // fixed price
        weight_info: {  // this is purely informational
            approx_weight: 1.4,
            unit: "KG",
            price_per_unit: 100
        }
    }]
}
```

## Picker

#### Fixed Price Items

For fixed price items, there is no special handling needed. They are treated like regular items.

#### Variable Price Items

For variable items, the pickers App plays an important role in suppling the actual weight of the items. Suki server then uses the actual weight information to recalcuate the unit price. If the there more than one items, the unit price is the average of all the items.

See example below:

Pickers submits the actual weight and final quantity.

```json5
{   lines: [{
        barcode: "111"
        quantity: 1,
        weight: 1.5
    }]
}
```

Suki server recalculates the price based on the weight info.

```json5
{   lines: [{
        barcode: "111"
        quantity: 1,
        weight: 1.5,
        weight_info: {
            approx_weight: 1.4,
            unit: "KG",
            price_per_unit: 100
            variable: true
        },
        price: 150
    }]
}
```

If quantity is more than, picker can optionally provide a list of weights as shown in example below.

```json5
{   lines: [{
        barcode: "111"
        quantity: 2,
        weights: [1.3,1.4]
        weight: 2.7
    }]
}
```

## API

### Mobile Client - Downloading products

```
GET /c/api/products
```

### Picker - Submitting Orders

```
PUT /c/api/orders/{order_id}
```
