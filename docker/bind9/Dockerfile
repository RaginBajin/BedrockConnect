FROM ubuntu:jammy

RUN apt update \
  && apt dist-upgrade -y \
  && apt install -y wget

# Create the folder for bind scripts
RUN mkdir -p /docker/bind

# Download the install script
RUN wget -O /docker/bind/docker-install-bind.sh \
  https://raw.githubusercontent.com/Pugmatt/BedrockConnect/master/scripts/docker-install-bind.sh

# Make it executable
RUN chmod +x /docker/bind/docker-install-bind.sh

# Provide default values for environment variables (optional)
ENV BCIP=""
ENV NSIP=""

# At container startup, pass BCIP/NSIP from environment to the script
CMD ["/bin/bash", "-c", "/docker/bind/docker-install-bind.sh ${BCIP} ${NSIP} \
  && /etc/init.d/named start \
  && while :; do sleep 10; done"]
