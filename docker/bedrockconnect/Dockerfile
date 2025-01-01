ARG BUILD_FROM=openjdk:24-ea-jdk-slim-bookworm
FROM ${BUILD_FROM}

ARG APT_UPDATE=20240910.1 \
    TARGETARCH 

ARG USERNAME=bedrock
ARG USER_UID=3000
ARG USER_GID=$USER_UID

# renovate: datasource=github-releases depName=itzg/easy-add versioning=loose
ARG BIN_EASY_ADD_VERSION=0.8.8

# renovate: datasource=github-releases depName=itzg/entrypoint-demoter versioning=loose extractVersion=(^v(?<version>.*)$)
ARG BIN_ENTRYPOINT_DEMOTER_VERSION=0.4.6

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

RUN mkdir -p /config && chmod 775 /config \
    && groupadd --gid "$USER_GID" "$USERNAME" \
    && useradd --uid "$USER_UID" --gid "$USER_GID" -m "$USERNAME" \
    && chown "$USERNAME:$USERNAME" /config

ADD https://github.com/itzg/easy-add/releases/download/${BIN_EASY_ADD_VERSION}/easy-add_linux_${TARGETARCH} \
    /usr/local/bin/easy-add

RUN chmod +x /usr/local/bin/easy-add \
&& easy-add --var version="${BIN_ENTRYPOINT_DEMOTER_VERSION}" \
    --var app=entrypoint-demoter \
    --file {{.app}} \
    --from https://github.com/itzg/{{.app}}/releases/download/v{{.version}}/{{.app}}_{{.version}}_linux_${TARGETARCH}.tar.gz

COPY *.sh /opt/
RUN chmod +x /opt/bedrock-entry.sh

USER $USERNAME

WORKDIR /home/$USERNAME

ENTRYPOINT ["/usr/local/bin/entrypoint-demoter", "--match", "/home/bedrock", "--debug", "--stdin-on-term", "stop", "/opt/bedrock-entry.sh"]

LABEL \
    org.opencontainers.image.title="raginbajin/minecraft-bedrock-connect" \
