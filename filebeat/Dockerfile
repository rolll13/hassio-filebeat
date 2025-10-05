ARG BUILD_FROM
FROM $BUILD_FROM

COPY rootfs /

ARG BUILD_ARCH

COPY /share/docker_images/filebeat-8.2.3-linux-arm64.tar.gz /tmp/filebeat.tar.gz

RUN tar -xzf /tmp/filebeat.tar.gz -C /tmp \
    && mv /tmp/filebeat*/filebeat /bin/filebeat \
    && chmod +x /bin/filebeat

CMD [ "/run.sh" ]
