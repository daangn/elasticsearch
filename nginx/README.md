# Elasticsearch Proxy with Nginx

### Usage

```
docker run -d -p 80:80 \
  -e SEARCH_USER='xxx' \
  -e SEARCH_PASSWORD_ENCRYPTED='encrypted password' \
  -e ELASTICSEARCH_PORT_9200_TCP_ADDR='' \
  -e ELASTICSEARCH_PORT_9200_TCP_PORT='' \
  n42corp/elasticsearch-proxy-nginx
```

or link containers

```
docker run -d -p 80:80 \
  --link elasticsearch \
  -e SEARCH_USER='xxx' \
  -e SEARCH_PASSWORD_ENCRYPTED='encrypted password' \
  n42corp/elasticsearch-proxy-nginx
```

#### How to get `SEARCH_PASSWORD_ENCRYPTED`

```
$ htpasswd -nb username password
username:$apr1$hs8saJAm$FQRhTA20aJM5W2kUXsdey0
```

`$apr1$hs8saJAm$FQRhTA20aJM5W2kUXsdey0` is `SEARCH_PASSWORD_ENCRYPTED`

### Build

  docker build -t n42corp/elasticsearch-proxy-nginx .
