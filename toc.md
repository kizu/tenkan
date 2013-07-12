# Helper for creating TOC from a page

Easy getting a substring at the given index with the given length.

Accepts the content in markdown: `toc_input`, returns two arrays: `toc_titles` and `toc_links`.

Check if the post is in markdown, convert if needed

``` django
{% assign toc_input = toc_input.content %}
{% assign toc_test_content = toc_input | prepend:'^' %}
{% if toc_test_content contains "^#" %}
    {% assign toc_input = toc_input | markdownify %}
{% endif %}
```


``` django
{% assign toc_headers_id = '' %}
{% assign toc_headers_indexes = '' %}
{% assign toc_headers = toc_input | split:'<h' %}

{% assign toc_index = 0 %}
{% for toc_header in toc_headers %}
    {% if toc_index > 0 %}
        {% assign toc_header_index = toc_header | truncate:1,'' %}
        {% assign indexof_input = toc_header %}
        {% assign indexof_substr = 'id="' %}
        {% include tenkan/indexof.md %}
        {% assign toc_header_id_start = indexof_result | plus:4 %}

        {% assign indexof_input = toc_header %}
        {% assign indexof_substr = '">' %}
        {% include tenkan/indexof.md %}
        {% assign toc_header_id_end = indexof_result %}

        {% assign substr_input = toc_header %}
        {% assign substr_index = toc_header_id_start %}
        {% assign substr_length = toc_header_id_end | minus:toc_header_id_start %}
        {% include tenkan/substr.md %}
        {% if substr_result != false %}
            {% capture toc_headers_id %}{{ toc_headers_id }}, {{ substr_result }}{% endcapture %}
            {% capture toc_headers_indexes %}{{ toc_headers_indexes }}, {{ toc_header_index }}{% endcapture %}
        {% endif %}
    {% endif %}
    {% assign toc_index = toc_index | plus:1 %}
{% endfor %}
{% assign toc_headers_id = toc_headers_id | split:', ' %}
{% assign toc_headers_indexes = toc_headers_indexes | split:', ' %}
```
