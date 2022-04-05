# Section & Schema setting states

The below table details what each setting returns.

* _Empty_ means the setting has never been set
* _Cleared_ means it's been set and then cleared
* Each heading means that the setting `==` the heading, e.g. `number == ''` or `image_picker == blank`
* Based on the condition being `true` a check is rendered, and a cross if it's `false`

## Results

|**Name**|**Type**|`''`|`false`|`nil`|`empty`|`blank`|
|--- |--- |--- |--- |--- |--- |--- |
|number|empty|❌|❌|✅|❌|✅|
||cleared|❌|❌|✅|❌|✅|
|radio|empty|❌|❌|❌|❌|❌|
||cleared|❌|❌|❌|❌|❌|
|range|empty|❌|❌|❌|❌|❌|
||cleared|❌|❌|❌|❌|❌|
|select|empty|❌|❌|❌|❌|❌|
||cleared|❌|❌|❌|❌|❌|
|text|empty|✅|❌|❌|✅|✅|
||cleared|✅|❌|❌|✅|✅|
|textarea|empty|✅|❌|❌|✅|✅|
||cleared|✅|❌|❌|✅|✅|
|article|empty|✅|❌|✅|✅|✅|
||cleared|✅|❌|✅|✅|✅|
|blog|empty|✅|❌|✅|✅|✅|
||cleared|✅|❌|✅|✅|✅|
|collection|empty|✅|❌|✅|✅|✅|
||cleared|✅|❌|✅|✅|✅|
|collection_list|empty|❌|❌|❌|✅|✅|
||cleared|❌|❌|❌|✅|✅|
|html|empty|✅|❌|❌|✅|✅|
||cleared|✅|❌|❌|✅|✅|
|image_picker|empty|❌|❌|✅|❌|✅|
||cleared|❌|❌|✅|❌|✅|
|link_list|empty|✅|❌|✅|✅|✅|
||cleared|✅|❌|✅|✅|✅|
|liquid|empty|❌|❌|✅|❌|✅|
||cleared|❌|❌|✅|❌|✅|
|page|empty|✅|❌|✅|✅|✅|
||cleared|✅|❌|✅|✅|✅|
|product|empty|✅|❌|✅|✅|✅|
||cleared|✅|❌|✅|✅|✅|
|product_list|empty|❌|❌|❌|✅|✅|
||cleared|❌|❌|❌|✅|✅|
|richtext|empty|✅|❌|❌|✅|✅|
||cleared|✅|❌|❌|✅|✅|
|url|empty|❌|❌|✅|❌|✅|
||cleared|❌|❌|✅|❌|✅|
|video_url|empty|❌|❌|✅|❌|✅|
||cleared|❌|❌|✅|❌|✅|

## Notes

* Number can't be cleared once set, it defaults to `0`
* Radio, range, and select always have a value
