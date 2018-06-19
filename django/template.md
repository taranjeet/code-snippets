## Django templates


#### Iterate over form in template in generic way

Uses django-widget-tweak to add bootstrap(v4) classes.

```
{% load widget_tweaks %}

<form method="post" enctype="multipart/form-data">
{% for hidden in form.hidden_fields %}
    {{hidden}}
{% endfor %}
{% for field in form.visible_fields %}
    <div class="form-group">
        <label class="form-label" for="{{ field.id_for_label }}">
            {{ field.label }}
        </label>
        {{ field|add_class:'form-control'|add_error_class:'is-invalid' }}
        {% if field.errors %}
        <div class="invalid-feedback">
            {{field.errors}}
        </div>
        {% endif %}
    </div>

{% endfor %}
<button type="submit" class="btn btn-primary ml-auto">Submit</button>
</form>
```

#### Iterate messages in template

```
# message.html
{% for message in messages %}
<div class="alert {% if message.tags %} {% if message.tags == 'error' %}alert-danger{% else %}alert-{{ message.tags }}{% endif %}{% endif %} alert-dismissible" role="alert">
    <button type="button" class="close" data-dismiss="alert" aria-label="Close"><span aria-hidden="true">&times;</span></button>
    {{message}}
</div>
{% endfor %}
```

#### Write a template tag

```
# in any app, create the following structure

├── templatetags
│   ├── __init__.py
│   └── app_extras.py
```

```
# app_extras.py

from django import template

register = template.Library()

# to lookup any value from dict object in template
@register.filter(name='lookup')
def cut(dict_obj, key):
    if isinstance(dict_obj, dict):
        return dict_obj.get(key, {})
    else:
        return dict_obj

```

```
# app_template.html

{% load app_extras %}

{{my_dict_object|lookup:"custom_key"}}

```
