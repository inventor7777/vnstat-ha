FROM ghcr.io/home-assistant/base:latest

RUN apk add --no-cache \
    iproute2 \
    python3 \
    vnstat

WORKDIR /app

COPY run.sh /run.sh
COPY app /app

RUN chmod a+x /run.sh

CMD ["/run.sh"]
