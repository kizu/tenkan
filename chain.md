{% capture null %}

    {% assign chain = include.chain %}
    {% assign joined_chain = include.chain | join:'_' %}
    {% if chain == joined_chain %}
        {% assign chain = chain | split:',' %}
    {% endif %}
    {% assign chain_buffer = include.content %}
    {% for step in chain %}
        {% capture chain_include %}{{ step | replace:' ' }}.md{% endcapture %}
        {% include {{chain_include}} content=chain_buffer %}
    {% endfor %}

{% endcapture %}{{ chain_buffer }}