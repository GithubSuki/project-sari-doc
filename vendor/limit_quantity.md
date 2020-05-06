# Limiting Quantities In Mobile App

You can restrict the maximum quantity of an item per transaction. You can also limit the maximum quantity of items under a group.

Suki provides the capability to capture the neccessary information for this feature to function, while the actual implementation of the limitation rests with the mobile app.

## Applying Limit Per Item

To limit the quantity per item, simply upload a file containing `barcode` and `limit`.

Example 1: products.csvs

```
barcode,limit
111,10
222,5
333,10
```

In this example, we are limiting the item with barcode 111 to maximum of 10 quantities per transaction, and item 222 to 5 quantities per transaction.

If you would like to remove the limit, you need to re-upload the file containg `barcode`
and `limit`, but this time, set the `limit` to empty field. See sample file here.

```
barcode,limit
111,
222,
333,
```

## Apply Limit to a Group, using Special tags

To apply limit to a group of products, first you have tag the items with special_tags.
You can do so by uploading a file with columns `barcode`, `special_tags`.

Example 2: products.csvs

```
barcode,special_tags
111,Shampoo
222,Shampoo
333,Can Goods
```

Afterwhich, you need to upload a file a table containg the tags (special) and their limits.

Example 3: limit_by_special_tags.cvs

```
tag_name,limit
Shampoo,5
Can Goods,10
```

At the end, you have to turn on the `enable_limit_quantity setting`; so the mobile app knows that it needs to download the limit_table and honor the settings.
