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
