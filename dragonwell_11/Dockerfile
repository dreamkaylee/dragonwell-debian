FROM centos:7 as jlink-builder

# ARG DRAGONWELL_URL=

WORKDIR /opt
RUN yum install -y wget && \
    wget -O jdk.tar.gz $DRAGONWELL_URL && \
    tar zxf jdk.tar.gz && \
    ln -s dragonwell-* java && \
    rm -f jdk.tar.gz

RUN /opt/java/bin/jlink \
    --module-path /opt/java/jmods \
    --compress=2 \
    --add-modules jdk.charsets,jdk.jfr,jdk.management.agent,java.base,java.logging,java.xml,jdk.unsupported,java.sql,java.sql.rowset,java.naming,java.desktop,java.management,java.security.jgss,java.instrument \
    --no-header-files \
    --no-man-pages \
    --strip-debug \
    --output /opt/jdk-11-mini-runtime

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

COPY --from=jlink-builder /opt/jdk-11-mini-runtime /usr/local/java
RUN ln -s $JAVA_HOME/bin/java /usr/bin/java
