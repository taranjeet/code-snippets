## Django models


#### Override model save

```
from django.db import models


class Author(models.Model):

    first_name = models.CharField(max_length=255)
    last_name = models.CharField(max_length=255)
    full_name = models.CharField(max_length=255)

    def __str__(self):
        return '{} - {}'.format(self.first_name, self.last_name)

    def save(self, *args, **kwargs):
        self.full_name = '{} {}'.format(self.first_name, self.last_name)
        super(Book).save(*args, **kwargs)
```

#### Create a new model property

```
from django.db import models


class Author(models.Model):

    first_name = models.CharField(max_length=255)
    last_name = models.CharField(max_length=255)

    def __str__(self):
        return '{} - {}'.format(self.first_name, self.last_name)

    def get_full_name(self):
        return '{} {}'.format(self.first_name, self.last_name)
```

#### Create a new cached property in model

```
from django.db import models
from django.utils.functional import cached_property


class Author(models.Model):

    first_name = models.CharField(max_length=255)
    last_name = models.CharField(max_length=255)

    def __str__(self):
        return '{} - {}'.format(self.first_name, self.last_name)

    @cached_property
    def get_full_name(self):
        return '{} {}'.format(self.first_name, self.last_name)
```
