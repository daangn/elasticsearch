# Kibana

### Usage

```
docker run -d -p 5601:5601 \
  -e ELASTICSERACH_URL='http://localhost:9200' \
  n42corp/kibana
```

### Build

  docker build -t n42corp/kibana .
