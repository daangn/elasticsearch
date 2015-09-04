## Elasticsearch with korean analyzer Dockerfile

### Usage

  docker run -d -p 9200:9200 n42corp/elasticsearch

### Build

  docker build -t n42corp/elasticsearch .

#### Attach persistent/shared directories

  1. Create a mountable data directory `<data-dir>` on the host.

  2. Start a container by mounting data directory and specifying the custom configuration file:

    ```sh
    docker run -d -p 9200:9200 -p 9300:9300 -v <data-dir>:/data n42corp/elasticsearch
    ```

After few seconds, open `http://<host>:9200` to see the result.

### Plugins

  - /_plugin/head
  - /_plugin/inquisitor
  - /_plugin/kopf
  - elasticsearch-cloud-aws

### Build with custom dic, synonym

```
$ touch Dockerfile.your
$ cat 'FROM n42corp/elasticsearch' > Dockerfile.your
$ touch servicecustom.csv # fill your custom dic
$ touch synonym.txt # fill your custom synonym
$ docker build -t your/imagename -f Dockerfile.your .
```

#### N42 custom dic, synonym

```
$ curl -O https://raw.githubusercontent.com/n42corp/search-ko-dic/master/servicecustom.csv
$ curl -O https://raw.githubusercontent.com/n42corp/search-ko-dic/master/synonym.txt
$ docker build -t n42corp/elasticsearch-n42 -f Dockerfile.n42 .
```

### docker-compose

```
$ docker-compose up -d
$ curl -XGET http://username:password@192.168.99.100/
```
