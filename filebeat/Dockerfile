ARG BUILD_FROM
FROM $BUILD_FROM

COPY rootfs /

ARG BUILD_ARCH

RUN set -x \
 && FB_VERSION="8.2.3" \
 && case "${BUILD_ARCH}" in \
      "aarch64") ARCH_DL="arm64" ;; \
      *) echo "Unsupported arch: ${BUILD_ARCH}" && exit 1 ;; \
    esac \
 && echo "Downloading Filebeat ${FB_VERSION} for ${ARCH_DL}" \
 && apt-get update && apt-get install -y curl ca-certificates tar \
 && curl -L --fail "http://192.168.68.200:33055/filebeat-8.2.3-linux-arm64.tar.gz" \
        -o /tmp/filebeat.tar.gz \
 && tar -xzf /tmp/filebeat.tar.gz -C /tmp \
 && mv /tmp/filebeat-${FB_VERSION}-linux-${ARCH_DL}/filebeat /bin/filebeat \
 && chmod +x /bin/filebeat \
 && rm -rf /tmp/* /var/lib/apt/lists/*

CMD [ "/run.sh" ]
