FROM openjdk:8-jre-slim-buster

WORKDIR /opt

ENV HADOOP_VERSION=3.2.0
ENV METASTORE_VERSION=3.0.0

RUN apt-get update && apt-get install -y netcat curl

ENV HADOOP_HOME=/opt/hadoop-${HADOOP_VERSION}
ENV HIVE_HOME=/opt/apache-hive-metastore-${METASTORE_VERSION}-bin

RUN curl -L https://downloads.apache.org/hive/hive-standalone-metastore-${METASTORE_VERSION}/hive-standalone-metastore-${METASTORE_VERSION}-bin.tar.gz | tar zxf - && \
    curl -L https://archive.apache.org/dist/hadoop/common/hadoop-${HADOOP_VERSION}/hadoop-${HADOOP_VERSION}.tar.gz | tar zxf - && \
    curl -L --output postgresql-42.4.0.jar https://jdbc.postgresql.org/download/postgresql-42.4.0.jar && \
    curl -L --output hive-exec-${METASTORE_VERSION}.jar https://repo1.maven.org/maven2/org/apache/hive/hive-exec/${METASTORE_VERSION}/hive-exec-${METASTORE_VERSION}.jar && \
    cp postgresql-42.4.0.jar ${HIVE_HOME}/lib/ && \
    cp hive-exec-${METASTORE_VERSION}.jar ${HIVE_HOME}/lib/ && \
    rm -rf postgresql-42.4.0.jar && \
    rm -rf hive-exec-${METASTORE_VERSION}.jar

RUN curl -L --output $HADOOP_HOME/share/hadoop/common/lib/gcs-connector-hadoop3-latest.jar https://storage.googleapis.com/hadoop-lib/gcs/gcs-connector-hadoop3-latest.jar
RUN apt-get -y install gettext-base

COPY scripts/entrypoint.sh /entrypoint.sh
COPY conf /opt/apache-hive-metastore-3.0.0-bin/conf

RUN groupadd -r hive --gid=1000 && \
    useradd -r -g hive --uid=1000 -d ${HIVE_HOME} hive && \
    chown hive:hive -R ${HIVE_HOME} && \
    chown hive:hive /entrypoint.sh && chmod +x /entrypoint.sh

USER hive
EXPOSE 9083

ENTRYPOINT ["sh", "-c", "/entrypoint.sh"]
