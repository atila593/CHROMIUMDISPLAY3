ARG BUILD_FROM=ghcr.io/home-assistant/amd64-base:3.20
FROM ${BUILD_FROM}

# Installation des paquets nécessaires (Wayland, Cage, Seatd, Navigateur, Outils de test)
RUN apk add --no-cache \
    cage \
    seatd \
    chromium \
    mesa-dri-gallium \
    mesa-gbm \
    udev \
    eudev \
    libinput \
    evtest \
    bash \
    tzdata \
    jq \
    dos2unix

# Copie des fichiers rootfs
COPY rootfs /

# Normalisation des fins de ligne et permissions des scripts
RUN if [ -d /etc/services.d ]; then \
        dos2unix /etc/services.d/*/run 2>/dev/null || true; \
        chmod -R 755 /etc/services.d/; \
    fi && \
    if [ -d /etc/cont-init.d ]; then \
        dos2unix /etc/cont-init.d/* 2>/dev/null || true; \
        chmod -R 755 /etc/cont-init.d/; \
    fi

ENTRYPOINT ["/init"]
