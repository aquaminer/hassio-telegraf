ARG BUILD_FROM=ghcr.io/hassio-addons/debian-base/amd64:9.1.0
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Setup base system
ARG BUILD_ARCH=amd64
ARG TELEGRAF_VERSION="1.37.0-1"
RUN \
    apt-get update \
    && apt-get install -y --no-install-recommends \
    iputils-ping=3:20221126-1+deb12u1 \
    snmp=5.9.3+dfsg-2 \
    lm-sensors=1:3.6.0-7.1 \
    smartmontools=7.3-1+b1 \
    ipmitool=1.8.19-4+deb12u2 \
    procps=2:4.0.2-3 \
    nvme-cli=2.4+really2.3-3 \
    \
    && ARCH="${BUILD_ARCH}" \
    && if [ "${BUILD_ARCH}" = "aarch64" ]; then ARCH="arm64"; \
    elif [ "${BUILD_ARCH}" = "armv7" ]; then ARCH="armhf"; fi \
    \
    && curl -J -L -o /tmp/telegraf.deb \
    "https://dl.influxdata.com/telegraf/releases/telegraf_${TELEGRAF_VERSION}_${ARCH}.deb" \
    \
    && dpkg --install /tmp/telegraf.deb \
    \
    && rm -fr \
    /tmp/* \
    /var/{cache,log}/* \
    /var/lib/apt/lists/*


# Copy root filesystem
COPY rootfs /


EXPOSE 8092/udp 8094 8125/udp 9273/tcp 25826/udp

# Build arguments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_DESCRIPTION
ARG BUILD_NAME
ARG BUILD_REF
ARG BUILD_REPOSITORY
ARG BUILD_VERSION

# Labels
LABEL \
    io.hass.name="${BUILD_NAME}" \
    io.hass.description="${BUILD_DESCRIPTION}" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="Oleksii Lozoviahin <github@aquaminer.me>" \
    org.opencontainers.image.title="${BUILD_NAME}" \
    org.opencontainers.image.description="${BUILD_DESCRIPTION}" \
    org.opencontainers.image.authors="Oleksii Lozoviahin <github@aquaminer.me>" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.source="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.documentation="https://github.com/${BUILD_REPOSITORY}/blob/main/README.md" \
    org.opencontainers.image.created=${BUILD_DATE} \
    org.opencontainers.image.revision=${BUILD_REF} \
    org.opencontainers.image.version=${BUILD_VERSION}
