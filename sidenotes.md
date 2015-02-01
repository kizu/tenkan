# Helper for creating sidenotes for a page

Create sidenotes from syntax like `<span class="sidenote" id="ID">Text for the sidenote (*  content of the sidenote)</span>`

Accepts the content in markdown: `sidenotes_input`, returns three arrays: `sidenotes_ids`, `sidenotes_contexts` and `sidenotes_contents`.

Use like this:

``` django
{% raw %}
{% endraw %}
```

Check if the post is in markdown, convert if needed

``` django
{% if sidenotes_input.content %}
    {% assign sidenotes_input = sidenotes_input.content %}
    {% assign sidenotes_test_content = sidenotes_input | prepend:'^' %}
    {% if sidenotes_test_content contains "^#" %}
        {% assign sidenotes_input = sidenotes_input | markdownify %}
    {% endif %}
{% endif %}
```


``` django
{% assign sidenotes_count = 0 %}
{% assign sidenotes_counter = 1 %}
{% assign sidenotes_ids = '' %}
{% assign sidenotes_id_strings = '' %}
{% assign sidenotes_contexts = '' %}
{% assign sidenotes_contents = '' %}
{% assign sidenotes_notes = sidenotes_input | split:'<span class="sidenote"' %}

{% for sidenotes_note in sidenotes_notes offset:1 %}
    {% assign sidenotes_note_test = sidenotes_note | truncate:1,'' %}
    {% if sidenotes_note_test == '>' %}
        {% capture sidenotes_note_id %}sidenote_{{ sidenotes_counter }}{% endcapture %}
        {% capture sidenotes_note_id_string %}<span class="sidenote">{% endcapture %}
        {% assign sidenotes_counter = sidenotes_counter | plus:1 %}
    {% else %}
        {% assign sidenotes_note_id = sidenotes_note | split:'">' | first | remove:' id="' %}
        {% capture sidenotes_note_id_string %}<span class="sidenote" id="{{ sidenotes_note_id }}">{% endcapture %}
    {% endif %}

    {% capture sidenotes_note_context_remove %}{{ sidenotes_note | split:'>' | first }}>{% endcapture %}
    {% assign sidenotes_note_context = sidenotes_note | remove_first:sidenotes_note_context_remove | split:' (* ' | first %}
    {% capture sidenotes_note_id_string %}{{ sidenotes_note_id_string }}{{ sidenotes_note_context }} (* {% endcapture %}

    {% capture sidenotes_note_content_remove %}{{ sidenotes_note | split:' (* ' | first }} (* {% endcapture %}
    {% assign sidenotes_note_content = sidenotes_note | remove_first:sidenotes_note_content_remove | split:')</span>' | first %}
    {% capture sidenotes_note_id_string %}{{ sidenotes_note_id_string }}{{ sidenotes_note_content }})</span>{% endcapture %}

    {% capture sidenotes_ids %}{{ sidenotes_ids }}, {{ sidenotes_note_id }}{% endcapture %}
    {% capture sidenotes_contexts %}{{ sidenotes_contexts }},,, {{ sidenotes_note_context }}{% endcapture %}
    {% capture sidenotes_contents %}{{ sidenotes_contents }},,, {{ sidenotes_note_content }}{% endcapture %}
    {% capture sidenotes_id_strings %}{{ sidenotes_id_strings }},,, {{ sidenotes_note_id_string }}{% endcapture %}

    {% assign sidenotes_count = sidenotes_count | plus:1 %}
{% endfor %}

{% assign sidenotes_ids = sidenotes_ids | remove_first:', ' | split:', ' %}
{% assign sidenotes_contexts = sidenotes_contexts | remove_first:',,, ' | split:',,, ' %}
{% assign sidenotes_contents = sidenotes_contents | remove_first:',,, ' | split:',,, ' %}
{% assign sidenotes_id_strings = sidenotes_id_strings | remove_first:',,, ' | split:',,, ' %}

{% if sidenotes_count == 0 %}
    {% if page.ensure_sidenote_layout %}
        {% assign sidenotes_count = 2 %}
    {% endif %}
{% endif %}
```
