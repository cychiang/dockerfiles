FROM debian:buster

ARG version=v1.0.2
ARG file_name=run.sh

RUN mkdir /app && \
    cd /app

RUN apt-get update && \
    apt-get install -y wget awscli && \
    wget https://github.com/otm/limes/releases/download/${version}/limes_linux_amd64 && \
    mv limes_linux_amd64 /usr/local/bin/limes && \
    chmod +x /usr/local/bin/limes

RUN echo "#!/bin/bash" \
    "\nip addr add 169.254.169.254/24 broadcast 169.254.169.255 dev lo:metadata" \
    "\nip link set dev lo:metadata up" \
    "\nsetcap 'cap_net_bind_service=+ep' /usr/local/bin/limes" \
    "\nlimes start" >> /app/run.sh && \
    chmod +x /app/run.sh
