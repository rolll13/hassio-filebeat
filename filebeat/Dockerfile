ARG BUILD_FROM
FROM $BUILD_FROM

COPY rootfs /

ARG BUILD_ARCH

RUN \
    FB_VERSIONS="8.2.3" && \
    find /opt -iname '*.yml' | xargs chmod 644 && \
    for ver in $FB_VERSIONS; do \
        echo "Downloading Filebeat version $ver for ${BUILD_ARCH}"; \
        case "${BUILD_ARCH}" in \
            "amd64")  ARCH_DL="x86_64" ;; \
            "armv7")  ARCH_DL="armv7" ;; \
            "aarch64") ARCH_DL="arm64" ;; \
            *) echo "Unsupported arch: ${BUILD_ARCH}" && exit 1 ;; \
        esac; \
        curl -sL --fail \
          "https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-8.2.3-linux-arm64.tar.gz" \
          -o /tmp/filebeat-${ver}.tar.gz && \
        tar -xzf /tmp/filebeat-${ver}.tar.gz -C /tmp && \
        mv /tmp/filebeat-${ver}-linux-${ARCH_DL}/filebeat /bin/filebeat-${ver} && \
        chmod +x /bin/filebeat-${ver} && \
        rm -rf /tmp/filebeat-${ver}*; \
    done && \
    rm -rf /env

CMD [ "/run.sh" ]
