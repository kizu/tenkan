# This file initializes most of the things you'll need from tenkan

Jekyll has a bit incostistent API for getting the page from `page` and any page from `site.posts` or similar, so for consistency we would work only with the latter, so for all the basic stuff we'll need the `processed_post` to be the current one.

``` django
{% assign get_title_input = page %}
{% for post in site.posts %}
    {% if page.id == post.id %}
        {% assign get_title_input = post %}
    {% endif %}
{% endfor %}
```

[Getting the title from markdown files](get_title.md), so we won't need to use `title` in YAML front matter.

``` django
{% include tenkan/get_title.md %}
{% assign processed_content = get_title_content %}
{% assign processed_title = get_title_output %}
```
