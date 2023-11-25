FROM golang:1.20 AS building

LABEL maintainer="lincoln@zocate.li"
LABEL version="1.0"

ARG exporter_version=1.7.0
ARG ARCH=amd64

RUN curl -LO https://github.com/prometheus/node_exporter/releases/download/v${exporter_version}/node_exporter-${exporter_version}.linux-${ARCH}.tar.gz \
    && tar -xvzf node_exporter-${exporter_version}.linux-${ARCH}.tar.gz  \
    && cp node_exporter-${exporter_version}.linux-${ARCH}/node_exporter /tmp/

FROM alpine:3.18

COPY --from=building /tmp/node_exporter /usr/local/bin/

EXPOSE 9100

ENTRYPOINT [ node_exporter --path.procfs=/usr/proc --path.sysfs=/usr/sys \
            --collector.filesystem.ignored-mount-points="^(/rootfs|/host|)/(sys|proc|dev|host|etc)($$|/)" \ 
            --collector.filesystem.ignored-fs-types="^(sys|proc|auto|cgroup|devpts|ns|au|fuse\.lxc|mqueue)(fs|)$$" ]

