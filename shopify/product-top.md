# product top

产品置顶

```
{%- liquid
    assign handles = ''
    assign handleStr = ''
    assign handles = collection.sort_by | downcase | replace:'%2C',',' | replace:'%2c',',' | split: ','
    for handle in handles
        assign product = all_products[handle]
        unless product == empty
            if handleStr.size > 0
                assign handleStr = handleStr | append: ',' | append: handle
            else
                assign handleStr = handleStr | append: handle
            endif
        endunless
    endfor
    for pr in collection.products
        if handles contains pr.handle
            continue
        else
            if handleStr.size > 0
                assign handleStr = handleStr | append: ',' | append: pr.handle
            else
                assign handleStr = handleStr | append: pr.handle
            endif
        endif
    endfor
    assign arrHandle = handleStr | split: ','
-%}
```

tip: 使用` all_products[handle] `的情况下，每个页面最多使用这种形式的产品解析限制为20.