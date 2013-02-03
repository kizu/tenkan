# Getting the title from markdown files

Jekyll have a bug (?) — the content of the posts in `site.posts` is rendered for the current page and all the older posts and is **not** rendered for all the newer posts, so when we need to get post's content, we need to look if it is still in markdown and then markdownify it.

Currently made hardcody, so it would work only if your post's content starts with a header.

    {% assign processed_content = processed_post.content %}
    {% assign test_content = processed_content | prepend:'^' %}
    {% if test_content contains "^#" %}
        {% assign processed_content = processed_content | markdownify %}
    {% endif %}

I couldn't find any other way to split by newlines except using the `newline_to_br`, `\n` didn't work here:

    {% assign content_lines = processed_content | newline_to_br | split:'<br />' %}

Check if the first line contains `# `, then set the `processed_title` to the found string and delete this header from the `processed_content`:

    {% if content_lines[0] contains '<h1 ' %}
        {% capture processed_title %}{{ content_lines[0] | strip_html }}{% endcapture %}
        {% assign processed_content = processed_content | remove_first:content_lines[0] %}
    {% else %}
        {% capture processed_title %}{{ page.title }}{% endcapture %}
    {% endif %}
