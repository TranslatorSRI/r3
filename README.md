# Redis-REST with referencing (R3)

Imagine a many-to-one mapping, e.g. synsets (terrible, horrible, no good, very bad)... For large synsets, storing this mapping naively is wasteful. Instead, assign each synset a unique ID. To look up the synset to which a term belongs, first look up the associated ID, then look up the (large) details about that synset.

This more efficient storage scheme is implemented as a Redis server with two logical databases, one for term→ID, the other for ID→synset.

This package assumes that such a Redis server is available and provides a REST server to perform both steps, allowing efficient term→synset lookups.

## Redis details

* the term→ID database is 0
  * values are assumed to be strings
* the ID→synset database is 1
  * values are assumed to be JSON-ified objects

## deployment

Assume a Redis database running on localhost port 6379.

### local

```bash
pip install -r requirements.txt
export REDIS_HOST=localhost
export REDIS_PORT=6379
./main.py --port 6380
```

### Docker

```bash
docker build -t redis_rest .
docker run \
    -p 6380:6380 \
    --env REDIS_HOST=host.docker.internal \
    --env REDIS_PORT=6379 \
    redis_rest --port 6380
```

### docker-compose

To run both Redis database and REST interface, a docker-compose file is provided:

```bash
docker-compose up
```

## usage

<http://localhost:6380/apidocs>
