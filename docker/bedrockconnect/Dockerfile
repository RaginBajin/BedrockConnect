ARG BUILD_FROM=openjdk:24-ea-jdk-slim-bookworm
FROM ${BUILD_FROM}

ARG USERNAME=bedrock
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Just install the packages you need, without version pinning
RUN apt-get update \
  && DEBIAN_FRONTEND=noninteractive apt-get purge -y \
       tar \
       perl \
       gcc-12 \
       nghttp2 \
       || true \
  && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
       curl \
       jq \
       nano \
       procps \
       sudo \
       unzip \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

RUN mkdir -p /config && mkdir -p /config && chmod 664 /config \
    && groupadd --gid "$USER_GID" "$USERNAME" \
    && useradd --uid "$USER_UID" --gid "$USER_GID" -m "$USERNAME" \
    && chown "$USERNAME:$USERNAME" /config

COPY *.sh /opt/

EXPOSE 19132/udp

USER $USERNAME

WORKDIR /home/$USERNAME

ENTRYPOINT ["/usr/local/bin/entrypoint-demoter", "--match", "/home/bedrock", "--debug", "--stdin-on-term", "stop", "/opt/bedrock-entry.sh"]

HEALTHCHECK --start-period=1m CMD ["/usr/local/bin/mc-monitor", "status-bedrock", "--host", "127.0.0.1", "--port", "19132"]

LABEL \
    org.opencontainers.image.title="raginbajin/minecraft-bedrock-connect" \
    org.opencontainers.image.description="Minecraft Bedrock Connect based on Pugmatt/BedrockConnect compatible with AMD64 and ARM64 (Rasp Pi)" \
    org.opencontainers.image.source="" \
    org.opencontainers.image.url="https://hub.docker.com/r/raginbajin/minecraft-bedrock-connect" \
    org.opencontainers.image.documentation="" \
    org.opencontainers.image.authors="Joe Bajin" \
    org.opencontainers.image.vendor="raginbajin"
