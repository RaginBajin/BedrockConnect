FROM openjdk:11
RUN apt-get update && apt-get install -y wget && rm -rf /var/lib/apt/lists/*
RUN mkdir -p /config
# Download the BedrockConnect jar
RUN wget -O /config/BedrockConnect.jar \
    https://github.com/Pugmatt/BedrockConnect/releases/latest/download/BedrockConnect-1.0-SNAPSHOT.jar

WORKDIR /config

# Define an environment variable for passing runtime options
ENV BRC_ARGS=""

CMD ["sh", "-c", "java -jar /config/BedrockConnect.jar $BRC_ARGS"]
