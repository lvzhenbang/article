# liquid

## Metafields

### metafields(list)

```
<div class="product__video-list">
    {%- for video in product.metafields.custom.suggested_video.value -%}
    {{ video | video_tag: loop: true, muted: true, controls: true }}
    {%- endfor -%}
</div>
```

处理` list `类型，使用 [` value `](https://shopify.dev/api/liquid/objects/metafield)
