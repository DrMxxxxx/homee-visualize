FROM grafana/grafana:7.5.5

USER root
RUN apk add tcpdump && \
    apk add curl && \
    apk add git && \
    apk upgrade

# ARG GF_UID="472"
# ARG GF_GID="472"

ENV PATH=/usr/share/grafana/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin \
    GF_PATHS_CONFIG="/etc/grafana/grafana.ini" \
    GF_PATHS_DATA="/var/lib/grafana" \
    GF_PATHS_HOME="/usr/share/grafana" \
    GF_PATHS_LOGS="/var/log/grafana" \
    GF_PATHS_PLUGINS="/var/lib/grafana/plugins" \
    GF_PATHS_PROVISIONING="/etc/grafana/provisioning" \
    GF_PATHS_DASHBOARDS="/var/lib/grafana/dashboards"

COPY ./run.sh ${GF_PATHS_DATA}/run.sh

#USER root
RUN addgroup -S grafana

RUN mkdir -p "$GF_PATHS_HOME/.aws" && \
    mkdir -p "$GF_PATHS_PROVISIONING/datasources" \
             "$GF_PATHS_PROVISIONING/dashboards" \
             "$GF_PATHS_PROVISIONING/notifiers" \
             "$GF_PATHS_DASHBOARDS" \
             "$GF_PATHS_LOGS" \
             "$GF_PATHS_DATA" && \
    #cp "/tmp/config/grafana.ini" "$GF_PATHS_CONFIG" && \
    chown -R grafana:grafana "$GF_PATHS_DATA" "$GF_PATHS_HOME/.aws" "$GF_PATHS_LOGS" "$GF_PATHS_PLUGINS" "$GF_PATHS_PROVISIONING" "$GF_PATHS_DASHBOARDS" && \
    chmod 777 -R "$GF_PATHS_DATA" "$GF_PATHS_HOME/.aws" "$GF_PATHS_LOGS" "$GF_PATHS_PLUGINS" "$GF_PATHS_PROVISIONING" "$GF_PATHS_DASHBOARDS"

RUN mkdir -p /data/grafana/plugins && chown -R grafana:grafana /data/grafana/plugins

RUN grafana-cli --pluginsDir "/data/grafana/plugins" plugins install grafana-piechart-panel && \
    grafana-cli --pluginsDir "/data/grafana/plugins" plugins install grafana-polystat-panel && \
    grafana-cli --pluginsDir "/data/grafana/plugins" plugins install grafana-clock-panel && \
    grafana-cli --pluginsDir "/data/grafana/plugins" plugins install michaeldmoore-multistat-panel && \
    grafana-cli --pluginsDir "/data/grafana/plugins" plugins install grafana-worldmap-panel && \
    grafana-cli --pluginsDir "/data/grafana/plugins" plugins install marcusolsson-hourly-heatmap-panel && \
    grafana-cli --pluginsDir "/data/grafana/plugins" plugins install cloudspout-button-panel && \
    grafana-cli --pluginsDir "/data/grafana/plugins" plugins install natel-discrete-panel && \
    grafana-cli --pluginsDir "/data/grafana/plugins" plugins install marcusolsson-dynamictext-panel && \
    grafana-cli --pluginsDir "/data/grafana/plugins" plugins install marcusolsson-treemap-panel && \
    grafana-cli --pluginsDir "/data/grafana/plugins" plugins install agenty-flowcharting-panel && \
    grafana-cli --pluginsDir "/data/grafana/plugins" plugins install marcusolsson-hourly-heatmap-panel && \
    grafana-cli --pluginsDir "/data/grafana/plugins" plugins install magnesium-wordcloud-panel && \
    grafana-cli --pluginsDir "/data/grafana/plugins" plugins install briangann-datatable-panel && \
    grafana-cli --pluginsDir "/data/grafana/plugins" plugins install aceiot-svg-panel && \
    grafana-cli --pluginsDir "/data/grafana/plugins" plugins install gapit-htmlgraphics-panel && \
    grafana-cli --pluginsDir "/data/grafana/plugins" plugins install novatec-sdg-panel && \
    grafana-cli plugins update-all

ENV GF_PATHS_PLUGINS="/data/grafana/plugins"

USER grafana
ENTRYPOINT ["/run.sh"]


# Todo: Just for debugging purposes, can be removed
# Make sure that the init flag is not there
#RUN rm -rf /var/lib/grafana/.init