# Helper for getting the substrings

Easy getting a substring at the given index with the given length.

Accepts three arguments: `substr_input`, `substr_index` and `substr_length`, returns `substr_result`.

``` django
{% if substr_length > 0 %}
    {% capture substr_temp %}{{ substr_input | truncate:substr_index,'' }}{% endcapture %}
    {% capture substr_temp %}{{ substr_input | remove_first:substr_temp }}{% endcapture %}
    {% capture substr_temp %}{{ substr_temp | truncate:substr_length,'' }}{% endcapture %}

    {% assign substr_result = substr_temp %}
{% endif %}

```

Reset the used arguments

``` django
{% assign substr_input = '' %}
{% assign substr_index = 0 %}
{% assign substr_length = 0 %}
```
