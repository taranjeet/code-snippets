## Custom Settings

### Analyzers

#### Store string as keyword analyzer

```
{
  "settings": {
    "index": {
      "analysis": {
        "analyzer": {
          "all_string_analyzer": {
            "filter": [
              "lowercase",
              "asciifolding",
              "trim"
            ],
            "char_filter": [
              "html_strip"
            ],
            "type": "custom",
            "tokenizer": "keyword"
          }
        }
      }
    }
  }
}
```
