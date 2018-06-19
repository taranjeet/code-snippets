## Queries

#### Completion Suggester: Suggest Query

```
{
  "author-suggest": {
    "prefix": "query here",
    "completion": {
      "field": "author"
    }
  }
}
```

#### Completion Suggester: Suggest Query with Context

```
{
  "author-suggest": {
    "prefix": "query here",
    "completion": {
      "field": "author",
      "contexts": {
        "genre": "genre here"
      }
    }
  }
}
```

#### Fuzzy query

```
{
  "query": {
    "match": {
      "author_name": {
        "query": "your query here",
        "fuzziness": 2,
        "operator": "and"
      }
    }
  }
}
```
