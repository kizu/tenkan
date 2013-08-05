# Helper for creating sidenotes for a page

Create sidenotes from syntax like `<sidenote id="ID">Text for the sidenote (* content of the sidenote)</sidenote>`

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
{% assign sidenotes_ids = '' %}
{% assign sidenotes_contexts = '' %}
{% assign sidenotes_contents = '' %}
{% assign sidenotes_notes = sidenotes_input | split:'<sidenote' %}

{% for sidenotes_note in sidenotes_notes offset:1 %}
    {% assign sidenote_note_id = sidenotes_note | split:'>' | first %}
    {% capture sidenotes_ids %}{{ sidenotes_ids }}, {{ sidenotes_note_id }}{% endcapture %}
{% endfor %}

{% assign sidenotes_ids = sidenotes_ids | split:', ' %}
{% assign sidenotes_contexts = sidenotes_contexts | split:',,, ' %}
{% assign sidenotes_contents = sidenotes_contents | split:',,, ' %}
```
