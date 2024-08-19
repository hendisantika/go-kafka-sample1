# go-kafka-sample1
=================

This repository contains simple examples of how to use the [franz-go](https://github.com/twmb/franz-go) library. The
repository accompanies the [blog post](https://aran.dev/posts/getting-started-with-golang-and-kafka).

## Running the examples

You'll first have to run the `docker-compose` file to start the Kafka compatible
broker [Redpanda](https://vectorized.io/redpanda).

```bash
❯ docker-compose -f bitnami.yml up
```

Then you can run the examples using the following commands:

To run the consumer:

```bash
❯ go ./consumer/main.go
```

To run the producer:

```bash
❯ go ./producer/main.go
```

### Sending notifications

With both producer and consumer running, you can simulate sending notifications. Open up a third terminal and use the
below curl commands to send notifications:

#### User 1 (Yuji) receives a notification from User 2 (Gojo):

```shell
curl -X POST http://localhost:8080/send \
-d "fromID=2&toID=1&message=Gojo started following you."
```

#### User 2 (Gojo) receives a notification from User 1 (Yuji):

```shell
curl -X POST http://localhost:8080/send \
-d "fromID=1&toID=2&message=Gojo mentioned you in a comment: 'Great seeing you yesterday, @Yuji!'"
```

#### User 1 (Yuji) receives a notification from User 4 (Geto):

```shell
curl -X POST http://localhost:8080/send \
-d "fromID=4&toID=1&message=Geto liked your post: 'My weekend getaway!'"
```

#### Retrieving notifications

Finally, you can fetch the notifications of a specific user. You can use the below curl commands to fetch notifications:

```shell
curl http://localhost:8081/notifications/1
```

```json
[
  {
    "notifications": [
      {
        "from": {
          "id": 2,
          "name": "Gojo"
        },
        "to": {
          "id": 1,
          "name": "Yuji"
        },
        "message": "Bruno started following you."
      },
      {
        "from": {
          "id": 2,
          "name": "Gojo"
        },
        "to": {
          "id": 1,
          "name": "Yuji"
        },
        "message": "Hendi started following you."
      },
      {
        "from": {
          "id": 4,
          "name": "Nanami"
        },
        "to": {
          "id": 1,
          "name": "Yuji"
        },
        "message": "Lena liked your post: 'My weekend getaway!'"
      }
    ]
  },
  "%"
]
```