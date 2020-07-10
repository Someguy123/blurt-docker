FROM debian:10

ARG BLURT_BINARY_URL="https://gitlab.com/blurt/blurt/-/jobs/632388178/artifacts/download?file_type=archive"
ENV BLURT_BINARY_URL ${BLURT_BINARY_URL}

ARG BLURT_WALLET_URL="https://gitlab.com/blurt/blurt/-/jobs/630888073/artifacts/download?file_type=archive"
ENV BLURT_WALLET_URL ${BLURT_WALLET_URL}

ARG BLURT_BINARY_NAME="blurtd"
ENV BLURT_BINARY_NAME ${BLURT_BINARY_NAME}

RUN apt-get update && \
    apt-get install -y unzip wget libbz2-dev libsnappy-dev libncurses5-dev libreadline-dev && \
    apt-get clean -qy

RUN cd /tmp && \
    echo " >>> Downloading blurtd binary: ${BLURT_BINARY_URL} ..." && \
    wget -O blurt.zip "${BLURT_BINARY_URL}" && \
    echo " >>> Unzipping blurt.zip ..." && \
    unzip blurt.zip && \
    echo " >>> Installing blurtd ..." && \
    cp "build/programs/blurtd/${BLURT_BINARY_NAME}" /usr/bin/blurtd && \
    echo "Cleaning up ..." && \
    rm -rvf build blurt.zip && \
    echo " >>> Downloading blurtd wallet: ${BLURT_WALLET_URL} ..." && \
    wget -O blurt.zip "${BLURT_WALLET_URL}" && \
    echo " >>> Unzipping blurt.zip ..." && \
    unzip blurt.zip && \
    cp "build/programs/cli_wallet/cli_wallet" /usr/bin/cli_wallet && \
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
