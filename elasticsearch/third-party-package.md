## Third Party Package

### Elasticdump

A [npm package](https://www.npmjs.com/package/elasticdump) to take and load dump of documents into elasticsearch

#### Take Dump

```
elasticdump --input=http://127.0.0.1:9200/author --output=author.json --type=data
```

#### Take Dump with Query

```
elasticdump --input=http://127.0.0.1:9200/author --output=author.json --type=data --searchBody '{"query": {"bool": {"filter": {"term": {"authorname": "AUTHOR_NAME_HERE"} } } } }'
```


#### Load Dump

```
elasticdump --input=author.json --output=http://127.0.0.1:9200/author --type=data
```

