## Miscellaneous

#### Write a management command

```
# in any app, create the following structure

├── management
│   ├── __init__.py
│   └── commands
│       ├── __init__.py
│       └── mynewmanagementcommand.py

```

```
# mynewmanagementcommand.py

from django.core.management.base import BaseCommand, CommandError


class Command(BaseCommand):

    help_text = 'A new management command for fun'
    can_import_settings = True

    def __init__(self, *args, **kwargs):
        super(Command, self).__init__(*args, **kwargs)
        # do whatever processing is required here

    def add_arguments(self, parser):
        parser.add_argument('argument_1', nargs='?',
                            help='Specify Argument 1 here')
        parser.add_argument('argument_2', nargs='?',
                            help='Specify Argument 2 here')

    def handle(self, *args, **options):
        if not options['argument_1'] or not options['argument_2']:
            raise CommandError("Option `--argument_1... --argument_2...` must be specified.")

        # do some processing here now
```
