# Getting the title from markdown files

Jekyll have a bug (?) — the content of the posts in `site.posts` is rendered for the current page and all the older posts and is **not** rendered for all the newer posts, so when we need to get post's content, we need to look if it is still in markdown and then markdownify it.

Currently made hardcody, so it would work only if your post's content starts with a header.

``` django
{% if get_title_input.content %}
    {% assign get_title_content = get_title_input.content %}
    {% assign get_title_test_content = get_title_content | prepend:'^' %}
    {% if get_title_test_content contains "^#" %}
        {% assign get_title_content = get_title_content | markdownify %}
    {% endif %}
{% else %}
    {% assign get_title_content = get_title_input %}
{% endif %}
```

I couldn't find any other way to split by newlines except using the `newline_to_br`, `\n` didn't work here:

``` django
{% assign content_lines = get_title_content | newline_to_br | split:'<br />' %}
```

Check if the first line contains `# `, then set the `get_title_output` to the found string and delete this header from the `get_title_content`:

``` django
{% if content_lines[0] contains '<h1 ' %}
    {% capture get_title_output %}{{ content_lines[0] | strip_html }}{% endcapture %}
    {% assign get_title_content = get_title_content | remove_first:content_lines[0] %}
{% else %}
    {% capture get_title_output %}{{ page.title }}{% endcapture %}
{% endif %}
```

And, finally, check for the current title, so we would use the explicitly set title instead of H1 one.

``` django
    {% assign name_from_url = get_title_input.url | split:'/' | last %}
    {% assign name_from_title = get_title_input.title | downcase | replace:' ','-' %}
    {% if get_title_input.title %}
        {% if name_from_url != name_from_title %}
            {% assign get_title_output = get_title_input.title %}
        {% endif %}
    {% endif %}
```
