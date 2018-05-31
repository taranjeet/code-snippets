## Django forms


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
