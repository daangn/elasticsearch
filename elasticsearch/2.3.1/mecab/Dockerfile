# from https://github.com/nacyot/elasticsearch
# The MIT License (MIT)
# Copyright (c) Dockerfile Project
#
FROM n42corp/es-base:2.3.1

RUN apt-get update
RUN apt-get install -y build-essential software-properties-common automake perl

# Install mecab-ko
RUN \
  cd /opt &&\
  wget https://bitbucket.org/eunjeon/mecab-ko/downloads/mecab-0.996-ko-0.9.2.tar.gz &&\
  tar xvf mecab-0.996-ko-0.9.2.tar.gz &&\
  cd /opt/mecab-0.996-ko-0.9.2 &&\
  ./configure &&\
  make &&\
  make check &&\
  make install &&\
  ldconfig

RUN \
  cd /opt &&\
  wget http://pkgs.fedoraproject.org/repo/pkgs/mecab-java/mecab-java-0.996.tar.gz/e50066ae2458a47b5fdc7e119ccd9fdd/mecab-java-0.996.tar.gz &&\
  tar xvf mecab-java-0.996.tar.gz &&\
  cd /opt/mecab-java-0.996 &&\
  sed -i 's|/usr/lib/jvm/java-6-openjdk/include|/usr/lib/jvm/java-8-openjdk-amd64/include|' Makefile &&\
  make &&\
  cp libMeCab.so /usr/local/lib

# Install mecab korean dic
RUN \
  cd /opt &&\
  wget https://bitbucket.org/eunjeon/mecab-ko-dic/downloads/mecab-ko-dic-2.0.1-20150920.tar.gz &&\
  tar xvf mecab-ko-dic-2.0.1-20150920.tar.gz &&\
  cd /opt/mecab-ko-dic-2.0.1-20150920 &&\
  ./autogen.sh &&\
  ./configure &&\
  make &&\
  make install

# Install mecab-ko-analyzer(elasticsearch plugin)
RUN /usr/share/elasticsearch/bin/plugin install https://bitbucket.org/eunjeon/mecab-ko-lucene-analyzer/downloads/elasticsearch-analysis-mecab-ko-2.3.1.0.zip

# Install elasticsearch plugin
RUN /usr/share/elasticsearch/bin/plugin install mobz/elasticsearch-head --verbose
RUN /usr/share/elasticsearch/bin/plugin install polyfractal/elasticsearch-inquisitor --verbose

# Install user dic
ONBUILD COPY servicecustom.csv /opt/mecab-ko-dic-2.0.1-20150920/user-dic/servicecustom.csv
ONBUILD RUN cd /opt/mecab-ko-dic-2.0.1-20150920 &&\
  tools/add-userdic.sh &&\
  make install

# Add synonym
ONBUILD COPY synonym.txt /usr/share/elasticsearch/config/synonym.txt
ONBUILD RUN chown elasticsearch.elasticsearch /usr/share/elasticsearch/config/synonym.txt

# Run Environment
ENV ES_JAVA_OPTS="-Des.security.manager.enabled=false -Djava.library.path=/usr/local/lib"
