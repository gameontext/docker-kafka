# Kafka and Zookeeper
FROM java:openjdk-8-jre

# Environment options
ENV DEBIAN_FRONTEND noninteractive
ENV SCALA_VERSION 2.12
ENV KAFKA_VERSION 0.10.2.0
ENV KAFKA_HOME /opt/kafka_"$SCALA_VERSION"-"$KAFKA_VERSION"
ENV PATH="${KAFKA_HOME}/bin:${PATH}"

# Install dependencies
RUN apt-get update && \
    apt-get install -y zookeeper wget supervisor dnsutils vim nano && \
    rm -rf /var/lib/apt/lists/* && \
    apt-get clean

# Install Kafka
RUN wget -q http://www-eu.apache.org/dist/kafka/"$KAFKA_VERSION"/kafka_"$SCALA_VERSION"-"$KAFKA_VERSION".tgz -O /tmp/kafka_"$SCALA_VERSION"-"$KAFKA_VERSION".tgz && \
    tar xfz /tmp/kafka_"$SCALA_VERSION"-"$KAFKA_VERSION".tgz -C /opt && \
    rm /tmp/kafka_"$SCALA_VERSION"-"$KAFKA_VERSION".tgz
    
# Start Kafka
COPY scripts/start-kafka.sh /usr/bin/start-kafka.sh

# Kafka config
RUN \
    sed -i "s|#delete.topic.enable|delete.topic.enable|g" "$KAFKA_HOME"/config/server.properties && \
    echo "/opt/kafka_${SCALA_VERSION}-${KAFKA_VERSION}/bin/kafka-topics.sh --list --zookeeper localhost:2181" > /test.sh && \
    chmod +x /test.sh
    

# Supervisor config
COPY supervisor/kafka.conf supervisor/zookeeper.conf /etc/supervisor/conf.d/

# 2181 is Zookeeper, 
# 9092 is Kafka
EXPOSE 2181 9092

# Start Supervisor
CMD ["supervisord", "-n"]

# Default directory for shell
WORKDIR $KAFKA_HOME