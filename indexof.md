# Helper for finding an index of a string

Get the index of a first accuarance of a given string in another string (if any)

Accepts three arguments: `indexof_input` and `indexof_substr`, returns `indexof_result`.

``` django
{% if indexof_substr != '' %}
    {% assign indexof_result = indexof_input | split:indexof_substr %}
    {% assign indexof_result = indexof_result[0] | size %}
{% endif %}

```

Reset the used arguments

``` django
{% assign indexof_input = '' %}
{% assign indexof_substr = '' %}
```
