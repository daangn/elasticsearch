FROM ubuntu:14.04

RUN \
  sed -i 's/# \(.*multiverse$\)/\1/g' /etc/apt/sources.list && \
  apt-get update && \
  apt-get -y upgrade && \
  apt-get install -y build-essential && \
  apt-get install -y software-properties-common && \
  apt-get install -y byobu curl git htop man unzip vim wget && \
  rm -rf /var/lib/apt/lists/*

# Install Java.
RUN \
  echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | debconf-set-selections && \
  add-apt-repository -y ppa:webupd8team/java && \
  apt-get update && \
  apt-get install -y oracle-java8-installer && \
  rm -rf /var/lib/apt/lists/* && \
  rm -rf /var/cache/oracle-jdk8-installer


# Install Kibana
ENV KIBANA_PKG_NAME kibana-4.1.1-linux-x64

RUN \
  cd /tmp && \
  wget https://download.elastic.co/kibana/kibana/$KIBANA_PKG_NAME.tar.gz && \
  tar xvzf $KIBANA_PKG_NAME.tar.gz && \
  mv /tmp/$KIBANA_PKG_NAME /kibana

COPY config/kibana.yml /kibana/config/kibana.yml

# https://github.com/kreuzwerker/envplate
COPY ep /usr/local/bin/ep
RUN chmod +x /usr/local/bin/ep

WORKDIR /kibana
EXPOSE 5601

CMD ["/usr/local/bin/ep", "/kibana/config/kibana.yml", "--", "/kibana/bin/kibana"]
