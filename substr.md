# Helper for getting the substrings

Easy getting a substring at the given index with the given length.

Accepts three arguments: `substr_input`, `substr_index` and `substr_length`, returns `substr_result`.

``` django
{% if substr_length > 0 %}
    {% assign substr_result = substr_input | truncate:substr_index,'' %}
    {% assign substr_result = substr_input | remove_first:substr_result %}
    {% assign substr_result = substr_result | truncate:substr_length,'' %}
{% endif %}
```

Reset the used arguments

``` django
{% assign substr_input = '' %}
{% assign substr_index = 0 %}
{% assign substr_length = 0 %}
```
