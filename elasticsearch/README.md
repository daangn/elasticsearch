# elasticsearch
- 공식 버전인 https://hub.docker.com/_/elasticsearch/ 과 거의 동일하지만 jdk 설치하는것이 다름,
- 공식이미지는 centos를 사용했지만 당근마켓의 자주사용하던 OS는 debian이기에 centos에서 debian으로 기본 OS 이미지를 변경하였다. 
- 이 repository에서는 이렇게 만들어진 이미지를 `base`라는 단어로 표현한다.
- 이 repository에서는 `base`를 제외한 나머지 특수한 것들이 인스톨 되어있는 이미지를 `specific`이라 한다.

# 버전 naming 설명

#### daangn/elasticsearch:{version}-{specific}
`base`이미지를 기반으로 하고 mecab인스톨 그리고 mecab plugin을 함께 인스톨한 이미지
- version: elasticsearch의 공식 버전
- specific: mecab이 달려있는지 달려있지 않은지, 또는 OS가 centos인지 아닌지 등에 대한 내용 `specific`은 복수개가 하나로 한다.

# Install & Run

``` bash
# (중요) base는 로컬에 이미지 이름을 달아서 이미지를 만들어 두어야 한다.
docker build -t daangn/elasticsearch:${VERSION} ${PROJECT_ROOT_PATH}/elasticsearch/${VERSION}/base/Dockerfile
# (선택) specific이미지들이 base이미지를 활용하기 때문에, base 이미지가 로컬에 반드시 있는 상태에서 수행 되어야 한다.
docker build -t daangn/elasticsearch:${VERSION}-${SPECIFIC} ${PROJECT_ROOT_PATH}/elasticsearch/${VERSION}/${SPECIFIC}/Dockerfile 

# 이미지가 잘 만들어 져 있는지에 대한 확인
docker images -a

# 특정 이미지 실행하기
# 1. docker-compose.yml을 작성해서 스크립트로 수행하거나...
# 2. 이렇게 노가다로 .. 터미널을 복붙해서 쓰거나...
docker run -d --name drakejin-elasticsearch -p 9200:9200 9300:9300 daangn/elasticsearch:${VERSION}-${SPECIFIC}
```


# Release note

### 2.3.1
- elastic search의 버전은 `2.3.1`
- OS는 `debian`, Docker 의 base image는 `openjdk:8-jdk`을 사용
- **생성된 이미지**: `daangn/elasticsearch:2.3.1`, `daangn/elasticsearch:2.3.1-mecab`

### 6.7.1
- elastic search의 버전은 `6.7.1`
- OS는 `debian`, Docker 의 base image는 `openjdk:8-jdk`을 사용
- **생성된 이미지**: `daangn/elasticsearch:6.7.1`, `daangn/elasticsearch:6.7.1-mecab`, `daangn/elasticsearch:6.7.1-official`
