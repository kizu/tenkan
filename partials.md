# Helper for including partials to a page

Adds partials to the given page

Check if the post is in markdown, convert if needed

    {% assign partials_result = '' %}
    {% if partials_input.partials %}
        {% assign sorted_pages = site.pages | sort:'path' %}
        {% for item in sorted_pages %}
            {% if item.path contains partials_input.partials %}
                {% assign item_id = item.path | split:'/' | last | remove:'.md' %}
                
                {% capture id_replacement_0 %}id="{{ item_id }}_0"{% endcapture %}
                {% capture id_replacement %}id="{{ item_id }}_{% endcapture %}

                {% capture partials_raw_content %}

{% unless partials_input.partials_no_ids %}{:#{{ item_id }}}{% endunless %}
{{ item.content }}

                {% endcapture %}
                {% capture partials_result %}{{ partials_result }}{{ partials_raw_content | markdownify | replace:'id="section"',id_replacement_0 | replace:'id="section-',id_replacement }}{% endcapture %}
            {% endif %}
        {% endfor %}

    {% endif %}
