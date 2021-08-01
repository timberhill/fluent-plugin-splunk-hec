FROM ruby:2.5.1-slim

LABEL maintainer="Splunk Inc. <DataEdge@splunk.com>"

# fluent-plugin-kubernetes-objects
ENV KO_VERSION=1.1.2
# fluent-plugin-k8s-metrics-agg
ENV KMA_VERSION=1.1.2
# fluent-plugin-kubernetes-metrics
ENV KM_VERSION=1.1.2
# fluent-plugin-splunk-hec
ENV SH_VERSION=1.1.0

# skip runtime bundler installation
ENV FLUENTD_DISABLE_BUNDLER_INJECTION 1

RUN set -e \
 && apt-get update \
 && apt-get upgrade -y \
 && apt-get install -y --no-install-recommends libjemalloc1 jq \
 && buildDeps="make gcc wget g++" \
 && apt-get install -y --no-install-recommends $buildDeps \
 && gem install -N fluentd -v "1.4.0" \
 && gem install -N fluent-plugin-systemd -v "1.0.2" \
 && gem install -N fluent-plugin-concat -v "2.3.0" \
 && gem install -N fluent-plugin-prometheus -v "1.3.0" \
 && gem install -N fluent-plugin-jq -v "0.5.1" \
 && gem install -N oj -v "3.5.1" \
 && gem install -N fluent-plugin-splunk-hec -v ${SH_VERSION} \
 && dpkgArch="$(dpkg --print-architecture | awk -F- '{ print $NF }')" \
 && apt-get install dumb-init \
 && apt-get purge -y --auto-remove \
                  -o APT::AutoRemove::RecommendsImportant=false \
                  $buildDeps \
 && rm -rf /var/lib/apt/lists/* \
 && rm -rf /tmp/* /var/tmp/* $GEM_HOME/cache/*.gem \
 && mkdir -p /fluentd/etc

COPY entrypoint.sh /fluentd/entrypoint.sh

ENTRYPOINT ["/bin/bash", "/fluentd/entrypoint.sh"]
CMD ["-h"]
