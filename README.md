## Elasticsearch with korean analyzer Dockerfile

### Usage

  docker run -d -p 9200:9200 -p 9300:9300 n42corp/elasticsearch:2.3.1.0

### Build

  docker build -t n42corp/elasticsearch:2.3.1.0 .

### Plugins

  - /_plugin/head
  - /_plugin/inquisitor

### Build with custom dic, synonym

사용자 사전과 동의어를 사용하려면 직접 Dockerfile을 만들고 해당 파일을 추가후 빌드

```
$ touch Dockerfile.your
$ cat 'FROM n42corp/elasticsearch:2.3.1.0' > Dockerfile.your
$ touch servicecustom.csv # fill your custom dic
$ touch synonym.txt # fill your custom synonym
$ docker build -t your/imagename -f Dockerfile.your .
```

#### N42 custom dic, synonym

당근마켓에서 사용하는 사용자 사전 및 동의어 사용하는 예제

```
$ curl -O https://raw.githubusercontent.com/n42corp/search-ko-dic/master/servicecustom.csv
$ curl -O https://raw.githubusercontent.com/n42corp/search-ko-dic/master/synonym.txt
$ docker build -t n42corp/elasticsearch-n42:2.3.1.0 -f Dockerfile.n42 .
```

N42 사용자 사전, 동의어로 빌드된것 실행

```
$ docker run -d -p 9200:9200 -p 9300:9300 n42corp/elasticsearch-n42:2.3.1.0
```

### 형태소 분석 테스트

임시 인덱스 만들고 거기에 anlayzer 및 동의어 필터 설정

```
$ curl -XPUT http://localhost:9200/korean/ -d '{
  "settings": {
    "index": {
      "analysis": {
        "analyzer": {
          "korean": {
            "type": "custom",
            "tokenizer": "mecab_ko_standard_tokenizer",
            "filter": ["synonym"]
          }
        },
        "filter": {
          "synonym": {
            "type": "synonym",
            "synonyms_path": "synonym.txt"
          }
        }
      }
    }
  }
}'
```

분석되는거 확인

```
$ curl -XGET "http://localhost:9200/korean/_analyze" -d '{"analyzer":"korean", "text":"나인봇"}'
```
