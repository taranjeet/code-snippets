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
