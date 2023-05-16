# Django admin 后台管理中显示图片 

## HTML文件

### 路径:D:\GitHub\Django\Lamps\Lib\site-packages\django\contrib\admin\templates\admin\widgets\clearable_file_input.html

#### 覆盖

```html
{% if widget.is_initial %}
    <p class="file-upload">
    {{ widget.initial_text }}:
    <a href="{{ widget.value.url }}">
        {{ widget.value }}
    </a>
    {% if not widget.required %}
        <span class="clearable-file-input">
        <input type="checkbox"
               name="{{ widget.checkbox_name }}"
               id="{{ widget.checkbox_id }}"
                {% if widget.attrs.disabled %} disabled {% endif %}>
        <label for="{{ widget.checkbox_id }}">
            {{ widget.clear_checkbox_label }}
        </label>
    </span>
    {% endif %}
    <br>
    {{ widget.input_text }}:{% endif %}
<input type="{{ widget.type }}"
       name="{{ widget.name }}"
        {% include "django/forms/widgets/attrs.html" %}>{% if widget.is_initial %}
    </p>
{% endif %}
{# 显示图片 #}
{% if widget.isImage %}
    <div style="max-width: 400px; max-height: 300px; margin: auto">
        <img src="{{ widget.value.url }}" style="max-width: 400px; max-height: 300px"/>
    </div>
{% endif %}
```

## python文件

### 路径:D:\GitHub\Django\Lamps\Lib\site-packages\django\forms\widgets.py

```python
class Widget(metaclass=MediaDefiningClass):
    def get_context(self, name, value, attrs):
        # 新增两行
        ImageFileList = {"png", "jpeg", "jpg", "gif", "svg", "webp", "bmp"}
        isImage = str(self.format_value(value)).rsplit(".")[-1] in ImageFileList
        
        return {
            'widget': {
                'name': name,
                'is_hidden': self.is_hidden,
                'required': self.is_required,
                'value': self.format_value(value),
                'attrs': self.build_attrs(self.attrs, attrs),
                'template_name': self.template_name,
                # 新增
                "isImage": isImage,
            },
        }
```



