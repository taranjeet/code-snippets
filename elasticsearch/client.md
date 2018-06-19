## Client

### Python

#### Scan and scroll

```
import json

from elasticsearch import Elasticsearch
from elasticsearch.helpers import scan

ES_HOST = 'http://localhost'
ES_PORT = 9202
ES_HOST_STRING = '{}:{}'.format(ES_HOST, ES_PORT)

es = Elasticsearch([ES_HOST_STRING])
query = '''{"query": {"match_all": {} }, "sort": [{"id": {"order": "asc"} } ]}'''
response = scan(es, query=query, size=100, index=INDEX_NAME, doc_type=TYPE_NAME)

for doc in response:
    parsed_doc = json.loads(json.dumps(doc))
    print parsed_doc.get('_id')
```

#### Compute shingled value of string for Completion Suggester

Used in completion suggester to store shingled value of string

```
def format_field_for_suggestion(text):
    text_list = text.split(' ')
    l = len(text_list)
    final_text_list = [text_list[x:l] for x in range(l)]
    final_text = [' '.join(x).lower() for x in final_text_list]
    return final_text

print format_field_for_suggestion("an example of completion suggestor")
['an example of completion suggestor', 'example of completion suggestor', 'of completion suggestor', 'completion suggestor', 'suggestor']
```

### Nodejs

#### Scan and scroll

```
# function which does scan and scroll
function scanAndScroll({ limit = 100, scrollId } = {}) {
    const query = {
        match_all: {},
    };

    // getHitsArray first gets hits.hits and then takes out source from each document
    const handler = result => {
        const scrollId = result._scroll_id;
        return { scrollId, hits: getHitsArray(result) };
    };

    if (scrollId) {
        console.log(`Found existing scroll id ${scrollId}. Hence Scrolling.`);
        return es.scroll({ scrollId, scroll: '10m' }).then(handler);
    }

    return es
        .search({
            index: indexName,
            type: typeName,
            body: { query, sort: [{ id: { order: 'asc' } }] },
            size: limit,
            scroll: '10m'
        })
        .then(handler);
}

# the one calling it

function callScan(scrollId) {

    return scanAndScroll({ scrollId })
    .then(response => {
        const scrollId = response.scrollId;
        if (response.hits.length === 0) {
            console.log('Zero hits found. Stopping scan and scroll');
            return [];
        }
        return doSomethingWithHits(response.hits)
        .then(() => {
            return callScan(scrollId);
        });
    });

}
```


