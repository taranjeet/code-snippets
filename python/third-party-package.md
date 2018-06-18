## Third Party Packages

### Pika(rabbitmq)

#### Create a connection

```
import pika

connection_string = 'amqp://{username}:{password}@{host}?heartbeat=60'.format(
    username=RABBIT_MQ_USERNAME, password=RABBIT_MQ_PASSWORD,
    host=RABBIT_MQ_HOST)
parameters = pika.connection.URLParameters(connection_string)
connection = pika.BlockingConnection(parameters)
channel = connection.channel()
```

#### Create a exchange and queue

```
import pika

connection_string = 'amqp://{username}:{password}@{host}?heartbeat=60'.format(
    username=RABBIT_MQ_USERNAME, password=RABBIT_MQ_PASSWORD,
    host=RABBIT_MQ_HOST)
parameters = pika.connection.URLParameters(connection_string)
connection = pika.BlockingConnection(parameters)
channel = connection.channel()
# create exchange
channel.exchange_declare(exchange=EXCHANGE_NAME,type=EXCHANGE_TYPE)
# create queue
channel.queue_declare(queue=QUEUE_NAME, durable=True)
# bind to queue by using binding key
channel.queue_bind(exchange=EXCHANGE_NAME,
    queue=QUEUE_NAME,
    routing_key='bindingkey.*.*'
)
```

#### Publish message

```
import json
import pika

connection_string = 'amqp://{username}:{password}@{host}?heartbeat=60'.format(
    username=RABBIT_MQ_USERNAME, password=RABBIT_MQ_PASSWORD,
    host=RABBIT_MQ_HOST)
parameters = pika.connection.URLParameters(connection_string)
connection = pika.BlockingConnection(parameters)
channel = connection.channel()
# create exchange
channel.exchange_declare(exchange=EXCHANGE_NAME,type=EXCHANGE_TYPE)
# create queue
channel.queue_declare(queue=QUEUE_NAME, durable=True)

# create message
message = {
    'key1': 'value1',
    'key2': 'value2',
}
channel.basic_publish(exchange=EXCHANGE_NAME,
    routing_key='routingkey.*.*',
    body=json.dumps(message),
    properties=pika.BasicProperties(delivery_mode=2)
)
```

### Requests

#### Get request with queryparams

```
import json
import requests

query_params = {
    'q1': 'value1',
    'q2': 'value2',
}

headers = {
    'Content-Type': 'application/json',
}

try:
    response = requests.get(url, data=json.dumps(query_params), headers=headers, timeout=10)
except Exception as e:
    print 'Failed to create request', e
    response = None

if response and response.status_code == requests.codes.ok:
    print '200 status code received'
```

#### Post request with request body

```
import json
import requests

post_data = {
    'key1': 'value1',
    'key2': 'value2',
}

headers = {
    'Content-Type': 'application/json',
}

try:
    response = requests.post(url, data=json.dumps(post_data), headers=headers)
except Exception as e:
    print 'Failed to create request', e
    response = None
```

#### Generic way to do both get/post request

```
import requests

post_data = {
    'key1': 'value1',
    'key2': 'value2',
}

headers = {
    'Content-Type': 'application/json',
}

method = 'POST'

req_args = {
    'data': post_data
}

try:
    response = requests.request(method, url, headers=headers, **req_args)
except Exception as e:
    print 'Failed to create request', e
    response = None
```
