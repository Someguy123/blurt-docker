FROM debian:10

RUN apt-get update && \
    apt-get install -y unzip curl wget jq net-tools dnsutils python3 python3-pip python3-dev python3-venv && \
    apt-get install -y liblz4-tool libbz2-dev libsnappy-dev libncurses5 libncurses5-dev libreadline-dev && \
    apt-get clean -qy

# BLURT_JOB references a gitlab job number to download binary artifacts from: https://gitlab.com/blurt/blurt/-/jobs
# full node job: 948413415
ARG BLURT_JOB="948413416"
ENV BLURT_JOB ${BLURT_JOB}

ARG BLURT_BINARY_URL="https://gitlab.com/blurt/blurt/-/jobs/${BLURT_JOB}/artifacts/download?file_type=archive"
ENV BLURT_BINARY_URL ${BLURT_BINARY_URL}

ARG BLURT_BINARY_PATH="build/bin/blurtd"
ENV BLURT_BINARY_PATH ${BLURT_BINARY_PATH}

ARG BLURT_WALLET_BINARY_PATH="build/bin/cli_wallet"
ENV BLURT_WALLET_BINARY_PATH ${BLURT_WALLET_BINARY_PATH}

RUN cd /tmp && \
    echo " >>> Downloading blurt binaries zip: ${BLURT_BINARY_URL} ..." && \
    wget -O blurt.zip "${BLURT_BINARY_URL}" && \
    echo " >>> Unzipping blurt.zip ..." && \
    unzip blurt.zip && \
    echo " >>> Installing blurtd ..." && \
    cp -v "${BLURT_BINARY_PATH}" /usr/bin/blurtd && \
#    echo "Cleaning up ..." && \
#    rm -rvf build blurt.zip && \
#    echo " >>> Downloading blurtd wallet: ${BLURT_WALLET_URL} ..." && \
#    wget -O blurt.zip "${BLURT_WALLET_URL}" && \
#    echo " >>> Unzipping blurt.zip ..." && \
#    unzip blurt.zip && \
    echo " >>> Installing cli_wallet ..." && \
    cp -v "${BLURT_WALLET_BINARY_PATH}" /usr/bin/cli_wallet && \
    echo "Cleaning up ..." && \
    rm -rvf build blurt.zip

# P2P (seed) port
EXPOSE 2001
EXPOSE 2015
# RPC ports
EXPOSE 5000
EXPOSE 8090
EXPOSE 8091

VOLUME /steem
WORKDIR /steem

ARG STEEMD_BIN="/usr/bin/blurtd"
ENV STEEMD_BIN ${STEEMD_BIN}

RUN echo "Please configure me! You need to mount a data directory onto /steem of this container to it to function correctly. (if you're using Steem-in-a-box most of this is handled automatically)"
CMD ["sh", "-c", "${STEEMD_BIN}"]

