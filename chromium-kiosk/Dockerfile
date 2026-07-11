ARG BUILD_FROM=ghcr.io/home-assistant/amd64-base:3.20
FROM ${BUILD_FROM}

# Installation de Chromium, des pilotes graphiques et du compositeur kiosque léger Cage
RUN apk add --no-cache \
    chromium \
    cage \
    mesa-dri-gallium \
    mesa-va-gallium \
    udev \
    jq \
    dos2unix

COPY rootfs /

# Nettoyage automatique des scripts d'initialisation pour S6-Overlay
RUN if [ -d /etc/services.d ]; then \
        dos2unix /etc/services.d/*/run 2>/dev/null || true; \
        chmod -R 755 /etc/services.d/; \
    fi

ENTRYPOINT ["/init"]
