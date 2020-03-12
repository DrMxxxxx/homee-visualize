FROM docker.elastic.co/logstash/logstash:7.6.0

COPY . .

USER root
RUN mkdir -p "/var/log/logstash" && \
    chown logstash:logstash /var/log/logstash && \
    bin/logstash-plugin install logstash-output-influxdb
