## Python General Code


#### Remove html tags from string

```
from HTMLParser import HTMLParser

class MLStripper(HTMLParser):
    def __init__(self):
        self.reset()
        self.fed = []

    def handle_data(self, d):
        self.fed.append(d)

    def get_data(self):
        return ' '.join(self.fed)


def strip_tags(html):
    s = MLStripper()
    s.feed(html)
    return s.get_data()

# now use strip_tags to remove html tags from string

```

#### Read from json file

```
def read_json_from_file(file_path):
    with open(file_path, 'r') as infile:
        return json.load(infile)
```

#### Write to json file

```
def write_json_to_file(file_path, token_map):
    with open(file_path, 'w') as f:
        json.dump(token_map, f)
```

#### Sort a dictionary

```
dict_as_list = sorted(dict_object.items(), key=lambda x: x[1], reverse=True)
```

#### Download file from url and save to a location

```
from os.path import join

import requests


def download_file(url, file_path):
    try:
        response = requests.get(url)
    except Exception as e:
        print 'Failed to download file from {} with error as {}'.format(url, e)
        return False
    if response.status_code == requests.codes.ok:
        with open(file_path, 'wb') as infile:
            infile.write(response.content)
        return True
    return False
```

