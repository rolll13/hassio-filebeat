ARG BUILD_FROM
FROM $BUILD_FROM

COPY rootfs /

ARG BUILD_ARCH

# === Шаг 1: Проверка окружения ===
RUN set -x && \
    echo ">> Step 1: Build architecture is ${BUILD_ARCH}" && \
    echo ">> Current working directory:" && pwd && \
    echo ">> Listing rootfs:" && ls -al /rootfs || true

# === Шаг 2: Настройка прав ===
RUN set -x && \
    echo ">> Step 2: Setting permissions for YAML files in /opt (if exists)" && \
    if [ -d /opt ]; then find /opt -iname '*.yml' | xargs chmod 644 || true; else echo "/opt not found"; fi

# === Шаг 3: Определяем архитектуру для загрузки ===
RUN set -x && \
    echo ">> Step 3: Detecting architecture..." && \
    case "${BUILD_ARCH}" in \
        "amd64")  ARCH_DL="x86_64" ;; \
        "armv7")  ARCH_DL="armv7" ;; \
        "aarch64") ARCH_DL="arm64" ;; \
        *) echo "Unsupported arch: ${BUILD_ARCH}" && exit 1 ;; \
    esac && \
    echo "${ARCH_DL}" > /arch.txt && \
    echo ">> ARCH_DL resolved to $(cat /arch.txt)"

# === Шаг 4: Скачивание Filebeat ===
RUN set -x && \
    FB_VERSION="8.2.3" && \
    ARCH_DL=$(cat /arch.txt) && \
    echo ">> Step 4: Downloading Filebeat ${FB_VERSION} for ${ARCH_DL}" && \
    URL="https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-${FB_VERSION}-linux-${ARCH_DL}.tar.gz" && \
    echo ">> Download URL: ${URL}" && \
    curl -v -L --fail "${URL}" -o /tmp/filebeat.tar.gz && \
    echo ">> Download completed. File size:" && ls -lh /tmp/filebeat.tar.gz

# === Шаг 5: Просмотр содержимого архива ===
RUN set -x && \
    echo ">> Step 5: Listing contents of /tmp/filebeat.tar.gz" && \
    tar -tzf /tmp/filebeat.tar.gz | head -n 50

# === Шаг 6: Распаковка и установка ===
RUN set -x && \
    FB_VERSION="8.2.3" && \
    ARCH_DL=$(cat /arch.txt) && \
    echo ">> Step 6: Extracting and installing Filebeat ${FB_VERSION} (${ARCH_DL})" && \
    tar -xzf /tmp/filebeat.tar.gz -C /tmp && \
    echo ">> Extracted folders:" && ls -R /tmp | head -n 50 && \
    FILEBEAT_BIN=$(find /tmp -type f -name "filebeat" | head -n 1) && \
    echo ">> Found binary at: ${FILEBEAT_BIN}" && \
    mv "${FILEBEAT_BIN}" /bin/filebeat && \
    chmod +x /bin/filebeat && \
    echo ">> Filebeat binary installed at /bin/filebeat" && \
    rm -rf /tmp/filebeat*

# === Шаг 7: Очистка ===
RUN set -x && \
    echo ">> Step 7: Cleaning up environment" && \
    rm -rf /env || true && \
    echo ">> Done!"

CMD [ "/run.sh" ]
