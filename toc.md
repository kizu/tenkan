# Helper for creating TOC from a page

Easy getting a substring at the given index with the given length.

Accepts the content in markdown: `toc_input`, returns two arrays: `toc_titles` and `toc_links`.

Use like this:

``` django
{% raw %}
    {% assign toc_input = page %}
    {% capture lolcache %}{% include tenkan/toc.md %}{% endcapture %}
    {% for toc_id in toc_headers_id %}
        {% if toc_headers_indexes[forloop.index0] != '' and toc_headers_indexes[forloop.index0] != '1' %}
            <a class="aside-nav-item{% if toc_headers_indexes[forloop.index0] != '2' %} aside-nav-item_{{ toc_headers_indexes[forloop.index0] | minus:1 }}{% endif %}" href="#{{ toc_id }}">{{ toc_headers_contents[forloop.index0] }}</a>
        {% endif %}
    {% endfor %}
{% endraw %}
```

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
{% assign toc_headers_contents = '' %}
{% assign toc_headers = toc_input | split:'<h' %}

{% for toc_header in toc_headers offset:1 %}
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

    {% capture toc_header_end %}</h{{ toc_header_index }}{% endcapture %}
    {% assign toc_header_content = toc_header | split:toc_header_end | first %}
    {% assign toc_header_content_remover = toc_header_content | split:'>' | first | append:'>' %}
    {% assign toc_header_content = toc_header_content | remove_first:toc_header_content_remover %}

    {% if substr_result != false %}
        {% capture toc_headers_id %}{{ toc_headers_id }}, {{ substr_result }}{% endcapture %}
        {% capture toc_headers_indexes %}{{ toc_headers_indexes }}, {{ toc_header_index }}{% endcapture %}
        {% capture toc_headers_contents %}{{ toc_headers_contents }},,, {{ toc_header_content | replace:'<a ','<span ' | replace:'</a>','</span>' }}{% endcapture %}
    {% endif %}
{% endfor %}
{% assign toc_headers_id = toc_headers_id | split:', ' %}
{% assign toc_headers_indexes = toc_headers_indexes | split:', ' %}
{% assign toc_headers_contents = toc_headers_contents | split:',,, ' %}
```
