FROM ghcr.io/home-assistant/base:latest

RUN apk add --no-cache \
    iproute2 \
    python3 \
    vnstat

WORKDIR /app

COPY run.sh /run.sh
COPY vnstat-cli /usr/local/bin/vnstat-cli
COPY app /app

RUN chmod a+x /run.sh /usr/local/bin/vnstat-cli

CMD ["/run.sh"]
