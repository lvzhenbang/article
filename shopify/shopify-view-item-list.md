# view_item_list

To track the "view_item_list" event in Shopify with GA4, you can use the following code snippet:

```
gtag('event', 'view_item_list', {
  'event_category': 'Ecommerce',
  'event_label': 'Product List',
  'items': [
    {
      'item_id': 'PRODUCT_ID_1',
      'item_name': 'Product Name 1',
      'item_brand': 'Product Brand 1',
      'item_category': 'Product Category 1',
      'item_variant': 'Product Variant 1',
      'item_list_name': 'Product List Name',
      'item_list_id': 'PRODUCT_LIST_ID'
    },
    {
      'item_id': 'PRODUCT_ID_2',
      'item_name': 'Product Name 2',
      'item_brand': 'Product Brand 2',
      'item_category': 'Product Category 2',
      'item_variant': 'Product Variant 2',
      'item_list_name': 'Product List Name',
      'item_list_id': 'PRODUCT_LIST_ID'
    }
    // Add more items if needed
  ]
});
```

Make sure to replace the placeholder values (PRODUCT_ID_1, Product Name 1, etc.) with the actual values for your products. You can include multiple items in the items array to track a list of products.

Remember to have the GA4 tracking code properly installed on your Shopify store for the event to be tracked correctly. Let me know if you have any further questions!