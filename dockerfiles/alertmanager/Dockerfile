FROM alpine:3.18

LABEL maintainer="lincoln@zocate.li"
LABEL version="1.0"

ARG alertmanager_version=0.26.0
ARG ARCH=amd64

WORKDIR /alertmanager

RUN apk --no-cache add curl \
    && curl -LO https://github.com/prometheus/alertmanager/releases/download/v${alertmanager_version}/alertmanager-${alertmanager_version}.linux-${ARCH}.tar.gz \
    && tar -xvzf alertmanager-${alertmanager_version}.linux-${ARCH}.tar.gz \
    && cp alertmanager-${alertmanager_version}.linux-${ARCH}/alertmanager /bin \
    && rm -rf alertmanager-${alertmanager_version}.linux-${ARCH}

VOLUME /alertmanager
VOLUME /etc/alertmanager

ADD config/config.yml /etc/alertmanager/

EXPOSE 9093

ENTRYPOINT ["alertmanager"] 

CMD ["--config.file=/etc/alertmanager/config.yml", "--storage.path=/alertmanager"]