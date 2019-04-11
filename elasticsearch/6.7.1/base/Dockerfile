# Official이미지와 다른것은
# - multiple stage dockerize를 이용하지 않았음. 
# - base image가 centos가 아닌 openjdk:8-jdk를 이용함
# - 때문에 package installer는 debian의 것을 이용

FROM openjdk:8-jdk

ENV PATH /usr/share/elasticsearch/bin:$PATH

RUN apt-get update
RUN apt-get install -y unzip curl wget
RUN apt-get clean all

# Replace OpenJDK's built-in CA certificate keystore with the one from the OS
# vendor. The latter is superior in several ways.
# REF: https://github.com/elastic/elasticsearch-docker/issues/171
RUN ln -sf /etc/pki/ca-trust/extracted/java/cacerts /usr/lib/jvm/java-8-openjdk-amd64/jre/lib/security/cacerts

RUN groupadd -g 1000 elasticsearch \
  && useradd -u 1000 -g 1000 -G 0 -d /usr/share/elasticsearch -ms /bin/bash elasticsearch

RUN chmod 0775 /usr/share/elasticsearch \
  && chgrp 0 /usr/share/elasticsearch

WORKDIR /usr/share/elasticsearch

RUN cd /opt && curl --retry 8 -s -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.7.1.tar.gz \
  && cd -\
  && tar zxf /opt/elasticsearch-6.7.1.tar.gz --strip-components=1

RUN grep ES_DISTRIBUTION_TYPE=tar /usr/share/elasticsearch/bin/elasticsearch-env \
  && sed -ie 's/ES_DISTRIBUTION_TYPE=tar/ES_DISTRIBUTION_TYPE=docker/' /usr/share/elasticsearch/bin/elasticsearch-env

RUN mkdir -p config data logs \
  && chmod 0775 config data logs

COPY config/elasticsearch.yml config/log4j2.properties config/
COPY --chown=1000:0 bin/docker-entrypoint.sh /usr/local/bin/docker-entrypoint.sh

# Openshift overrides USER and uses ones with randomly uid>1024 and gid=0
# Allow ENTRYPOINT (and ES) to run even with a different user
RUN chgrp 0 /usr/local/bin/docker-entrypoint.sh && \
    chmod g=u /etc/passwd && \
    chmod 0775 /usr/local/bin/docker-entrypoint.sh

EXPOSE 9200 9300

LABEL org.label-schema.schema-version="1.0" \
  org.label-schema.vendor="Elastic" \
  org.label-schema.name="elasticsearch" \
  org.label-schema.version="6.7.1" \
  org.label-schema.url="https://www.elastic.co/products/elasticsearch" \
  org.label-schema.vcs-url="https://github.com/elastic/elasticsearch" \
  license="Elastic License"

ENTRYPOINT ["/usr/local/bin/docker-entrypoint.sh"]
# Dummy overridable parameter parsed by entrypoint
CMD ["eswrapper"]