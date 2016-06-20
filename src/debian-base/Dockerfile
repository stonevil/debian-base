FROM debian:latest
MAINTAINER stonevil@gmail.com

ENV SOFTWARE_DOWNLOAD_TIMEOUT=${SOFTWARE_DOWNLOAD_TIMEOUT:-300}
ENV SOFTWARE_DOWNLOAD_RETRIES=${SOFTWARE_DOWNLOAD_RETRIES:-5}
ENV SOFTWARE_DOWNLOAD_RETRIES_DELAY=${SOFTWARE_DOWNLOAD_RETRIES_DELAY:-10}
ENV SOFTWARE_DOWNLOAD_CMD="curl --connect-timeout ${SOFTWARE_DOWNLOAD_TIMEOUT} --retry ${SOFTWARE_DOWNLOAD_RETRIES} --retry-delay ${SOFTWARE_DOWNLOAD_RETRIES_DELAY} --location"

ENV APT_UPDATE_CMD="apt-get update"
ENV APT_UPGRATE_CMD="apt-get -y --no-install-recommends upgrade"
ENV APT_INSTALL_CMD="apt-get -y --no-install-recommends install"
ENV APT_REMOVE_CMD="apt-get -y autoremove"
ENV APT_CACHE_RM="rm -rf /var/lib/apt/lists/* && rm -rf /tmp/*.deb && rm -rf /tmp/*.tar*"

ENV GOSU_VERSION=${GOSU_VERSION:-1.9}
ENV CONFD_VERSION=${CONFD_VERSION:-0.11.0}

LABEL architecture="amd64" \
  vendor=stonevil \
  com.stonevil.os="debian" \
  com.stonevil.version="latest" \
  com.stonevil.gosu.version=${GOSU_VERSION} \
  com.stonevil.confd.version=${CONFD_VERSION}

RUN ${APT_UPDATE_CMD} && ${APT_UPGRADE_CMD} && \
  ${APT_INSTALL_CMD} ca-certificates curl && \
  ${APT_CACHE_RM} && \
  echo 'hosts: files mdns4_minimal [NOTFOUND=return] dns mdns4' >> /etc/nsswitch.conf && \
  ${SOFTWARE_DOWNLOAD_CMD} -o /usr/bin/gosu https://github.com/tianon/gosu/releases/download/${GOSU_VERSION}/gosu-amd64 && chmod +x /usr/bin/gosu && \
  ${SOFTWARE_DOWNLOAD_CMD} -o /usr/bin/confd https://github.com/kelseyhightower/confd/releases/download/v${CONFD_VERSION}/confd-${CONFD_VERSION}-linux-amd64 && chmod +x /usr/bin/confd

CMD /bin/bash --version ; \
  confd --version ; \
  gosu
