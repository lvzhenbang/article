# PHP server-purchase for Shopify

使用`shopify flow` create-order send-http


## shopify flow

HTTP method `POST`
URL  https://megalook.store/ga

Headers opotion
```
Content-Type  application/json
```

Body data

```
{
  "client_id": "{{ order.customer.id | remove: 'gid://shopify/Customer/' }}",
  "event_data": {
    "transaction_id": "{{ order.name }}",
    "value": {{ order.currentTotalPriceSet.presentmentMoney.amount }},
    "tax": {{ order.currentTotalTaxSet.presentmentMoney.amount }},
    "shipping": {{ order.totalShippingPriceSet.presentmentMoney.amount }},
    "currency": "{{ order.currencyCode }}",
    "coupon": "{{ order.discountCode }}",
    "items": [{% for line_item in order.lineItems %}{
      "item_id": "{{ line_item.variant.product.id | remove:'gid://shopify/Product/' }}",
      "item_name": "{{ line_item.title | slice: 0, 90 }}",
      "item_sku":"{{ line_item.variant.sku }}",
      "currency": "{{ order.currencyCode }}",
      "coupon": "{{ order.discountCode }}",
      "discount": {{ line_item.totalDiscountSet.presentmentMoney.amount }},
      "price": {{ line_item.variant.price }},
      "quantity":{{ line_item.quantity }}
    }{% if forloop.index < forloop.length %},{% endif %}{% endfor %}]
  }
}
```

## php code

```
Route::post('/ga', function (Request $request) {
    $payLoad = json_decode($request->getContent(), true);
    Log::info($payLoad);
    $response = Http::withBody(json_encode([
            "client_id"=> $payLoad["client_id"],
            "non_personalized_ads"=> false,
            "events"=> [
                [
                    "name"=> "custom_purchase",
                    "params"=> $payLoad["event_data"],
                ]
            ]
        ]) ,'application/json')
        ->post('https://www.google-analytics.com/mp/collect?measurement_id=' . env('GA4_MEASUREMENT_ID') .'&api_secret='. env('GA4_API_SECRET'));
    // Log::info($response->json());
    // return $response->json();
});
```

[`Google Analytics Measurement Protocol`](https://developers.google.com/analytics/devguides/collection/protocol/ga4/reference/events#purchase)
