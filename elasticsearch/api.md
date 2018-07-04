## Api

#### Reindex Api

```
POST _reindex
{
  "source": {
    "index": "author_v1"
  },
  "dest": {
    "index": "author_v2"
  }
}
```

#### Add/Remove alias

```
POST /_aliases
{
    "actions" : [
        { "remove" : { "index" : "author_v1", "alias" : "author" } },
        { "add" : { "index" : "author_v2", "alias" : "author" } }
    ]
}
```
