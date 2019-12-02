From docker.elastic.co/logstash/logstash:7.3.1

COPY . .

USER root
RUN mkdir -p "/var/log/logstash" && \
    chown logstash:logstash /var/log/logstash && \
    bin/logstash-plugin install logstash-output-influxdb && \
    bin/logstash-plugin install logstash-output-graphite
