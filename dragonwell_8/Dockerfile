FROM centos:7 as jlink-builder

# ARG DRAGONWELL_URL=

WORKDIR /opt
RUN yum install -y wget \
    && wget -O jdk.tar.gz $DRAGONWELL_URL \
    && tar zxf jdk.tar.gz \
    && ln -s dragonwell-* java \
    && rm -f jdk.tar.gz \
    && rm -rf /opt/java/man \
           /opt/java/sample \
    && rm -f  /opt/java/src.zip
 
FROM debian:latest
LABEL maintainer="dreamkaylee@outlook.com"

ENV TZ=Asia/Shanghai

RUN sed -i 's@deb.debian.org@mirrors.ustc.edu.cn@g' /etc/apt/sources.list.d/debian.sources \
    && apt-get update

RUN apt-get install -y tzdata fontconfig unixodbc-dev \
    && rm -rf /var/lib/apt/lists/* \
    && echo $TZ > /etc/timezone \
    && ln -snf /usr/share/zoneinfo/$TZ /etc/localtime \
    && dpkg-reconfigure -f noninteractive tzdata

ENV JAVA_HOME=/usr/local/java
ENV PATH="$PATH:$JAVA_HOME/bin"

COPY --from=jlink-builder /opt/java /usr/local/java
RUN ln -s $JAVA_HOME/bin/java /usr/bin/java
