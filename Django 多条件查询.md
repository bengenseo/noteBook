# Django 多条件查询

```python
from web import models

from django.db.models import Q
text= models.ArcType.objects.filter(Q(reid=2)|Q(id=2))
```

