FROM grafana/grafana:7.1.0

USER root
RUN apk update && \
    apk upgrade

COPY config /tmp/config

ARG GF_UID="472"
ARG GF_GID="472"

ENV PATH=/usr/share/grafana/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin \
    GF_PATHS_CONFIG="/etc/grafana/grafana.ini" \
    GF_PATHS_DATA="/var/lib/grafana" \
    GF_PATHS_HOME="/usr/share/grafana" \
    GF_PATHS_LOGS="/var/log/grafana" \
    GF_PATHS_PROVISIONING="/etc/grafana/provisioning" \
    GF_PATHS_DASHBOARDS="/var/lib/grafana/dashboards" \
    GF_PANELS_DISABLE_SANITIZE_HTML=true \
    GF_SECURITY_ADMIN_USER="admin" \
    GF_SECURITY_ADMIN_PASSWORD="admin" \
    GF_USERS_DEFAULT_THEME="dark"

COPY ./run.sh ${GF_PATHS_DATA}/run.sh

USER root
RUN mkdir -p "$GF_PATHS_HOME/.aws" && \
    #groupadd -r -g $GF_GID grafana && \
    #useradd -r -u $GF_UID -g grafana grafana && \
    mkdir -p "$GF_PATHS_PROVISIONING/datasources" \
             "$GF_PATHS_PROVISIONING/dashboards" \
             "$GF_PATHS_PROVISIONING/notifiers" \
             "$GF_PATHS_DASHBOARDS" \
             "$GF_PATHS_LOGS" \
             "$GF_PATHS_DATA" && \
    # cp "$GF_PATHS_HOME/conf/sample.ini" "$GF_PATHS_CONFIG" && \
    # cp "/tmp/config/grafana.ini" "$GF_PATHS_CONFIG" && \
    # cp "$GF_PATHS_HOME/conf/ldap.toml" /etc/grafana/ldap.toml && \
    cp -r "/tmp/config/datasources/." "$GF_PATHS_PROVISIONING/datasources" && \
    # cp -r "/tmp/config/dashboards/." "$GF_PATHS_PROVISIONING/dashboards" && \
    # cp -r "/tmp/config/dashboards/files/." "$GF_PATHS_DASHBOARDS" && \
    chown -R grafana:grafana "$GF_PATHS_DATA" "$GF_PATHS_HOME/.aws" "$GF_PATHS_LOGS" "$GF_PATHS_PLUGINS" "$GF_PATHS_PROVISIONING" "$GF_PATHS_DASHBOARDS" && \
    chmod 777 -R "$GF_PATHS_DATA" "$GF_PATHS_HOME/.aws" "$GF_PATHS_LOGS" "$GF_PATHS_PLUGINS" "$GF_PATHS_PROVISIONING" "$GF_PATHS_DASHBOARDS"

RUN mkdir -p /data/grafana/plugins && chown -R grafana:grafana /data/grafana/plugins

RUN grafana-cli --pluginsDir "/data/grafana/plugins" plugins install grafana-piechart-panel && \
    grafana-cli --pluginsDir "/data/grafana/plugins" plugins install flant-statusmap-panel && \
    grafana-cli --pluginsDir "/data/grafana/plugins" plugins install fatcloud-windrose-panel && \
    grafana-cli --pluginsDir "/data/grafana/plugins" plugins install grafana-polystat-panel && \
    grafana-cli --pluginsDir "/data/grafana/plugins" plugins install grafana-clock-panel && \
    grafana-cli --pluginsDir "/data/grafana/plugins" plugins install andig-darksky-datasource && \
    grafana-cli --pluginsDir "/data/grafana/plugins" plugins install yesoreyeram-boomtable-panel && \
    grafana-cli plugins update-all

ENV GF_PATHS_PLUGINS="/data/grafana/plugins"

USER grafana
ENTRYPOINT ["/run.sh"]


# Todo: Just for debugging purposes, can be removed
# Make sure that the init flag is not there
#RUN rm -rf /var/lib/grafana/.init