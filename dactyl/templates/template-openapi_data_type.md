# {{title}}

{{description}}

{% if type is defined %}- **Type:** {{type|title}}
{% elif oneOf is defined %}- **Possible Types:**
{% for option in oneOf -%}
{% if option.enum is defined %}
    - One of the following {% if option.type is defined %}{{option.type|title}}s{% else %}values{% endif %}:
{% for suboption in option.enum %}
        - `{{suboption}}`
{% endfor -%}
{% else %}
    - {{option.type|title}}
{% endif -%}
{% endfor -%}
{% elif anyOf is defined %}- **Possible Types:**
{% for option in anyOf -%}
{% if option.enum is defined %}
    - Any of the following {% if option.type is defined %}{{option.type|title}}s{% else %}values{% endif %}:
{% for suboption in option.enum %}
        - `{{suboption}}`
{% endfor -%}
{% else %}
    - {{option.type|title}}
{% endif -%}
{% endfor -%}
{% elif allOf is defined %}- **Possible Types:**
{% for option in allOf -%}
{% if option.enum is defined %}
    - Must qualify as all of the following {% if option.type is defined %}{{option.type|title}}s{% else %}values{% endif %}:
{% for suboption in option.enum %}
        - `{{suboption}}`
{% endfor -%}
{% else %}
    - {{option.type|title}}
{% endif -%}
{% endfor -%}
{% endif -%}
{% if enum is defined %}- **Possible Values:**
{% for option in enum %}
    - `{{option}}`
{% endfor %}{% endif %}
{% if pattern is defined %}- **Pattern:** `{{pattern}}`
{% endif -%}
{% if example is defined %}- **Example:**

        {{example|indent(8,first=False)}}

{% endif -%}

{% if properties is defined %}
This type can contain the following fields:

| Field | Type | Required? | Description |
|-------|------|-----------|-------------|
{%- set required_fields = required if required is defined else [] -%}
{%- for name,field in properties.items() %}
| `{{name}}` | {{field.type|title}}{% if "items" in field.keys() and "title" in field["items"].keys() %} of [{{field["items"].title}}]({{type_link(field["items"].title)}}){% endif %} {% if field["title"] is defined %}([{{field.title}}]({{type_link(field.title)}})){% endif %} | {{"Required" if name in required_fields else "Optional"}} | {{field.description}} |
{%- endfor %}

{% if additionalProperties is defined and additionalProperties == True %}This type MUST NOT contain any additional fields.{% endif %}

{% elif type == "array" and items is defined %}
Each member of the array is a ***TODO*** {{items}}
{% endif %}
