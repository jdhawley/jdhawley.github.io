{% if page.additional_resources %}
    <h2>Additional Resources</h2>
    <ul>
        {% for resource in page.additional_resources %}
            <li>{{resource | markdownify}}</li>
        {% endfor %}
    </ul>
{% endif %}